---
title: "Konfigurace ověřování a autorizace pro vlastní aplikaci, která volá rozhraní API služby Azure časové řady Insights | Microsoft Docs"
description: "Tento kurz vysvětluje postup konfigurace ověřování a autorizace pro vlastní aplikaci, která volá rozhraní API služby Azure časové řady statistiky"
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
ms.openlocfilehash: 4dd4865dc556e09a31d2cb7a32768aeb19ba9900
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a><span data-ttu-id="73b3a-103">Ověřování a autorizace pro rozhraní API pro Azure časové řady přehledy</span><span class="sxs-lookup"><span data-stu-id="73b3a-103">Authentication and authorization for Azure Time Series Insights API</span></span>

<span data-ttu-id="73b3a-104">Tento článek vysvětluje, jak nakonfigurovat vlastní aplikaci, která volá rozhraní API služby Azure časové řady statistiky.</span><span class="sxs-lookup"><span data-stu-id="73b3a-104">This article explains how to configure a custom application that calls the Azure Time Series Insights API.</span></span>

## <a name="service-principal"></a><span data-ttu-id="73b3a-105">Instanční objekt</span><span class="sxs-lookup"><span data-stu-id="73b3a-105">Service principal</span></span>

<span data-ttu-id="73b3a-106">Tato část vysvětluje postup konfigurace aplikace pro přístup k rozhraní API pro přehledy časové řady jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="73b3a-106">This section explains how to configure an application to access the Time Series Insights API on behalf of the application.</span></span> <span data-ttu-id="73b3a-107">Aplikace můžete potom dotaz na data nebo publikování referenční data v prostředí časové řady Statistika s přihlašovací údaje aplikací a není pověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="73b3a-107">The application can then query data or publish reference data in the Time Series Insights environment with application credentials and not the user credentials.</span></span>

<span data-ttu-id="73b3a-108">Až budete mít aplikaci, která potřebuje přístup čas řady Insights, musíte nastavit aplikaci Azure Active Directory a přiřadit zásady přístupu k datům v prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="73b3a-108">When you have an application that needs to access Time Series Insights, you must set up an Azure Active Directory application and assign the data access policies in the Time Series Insights environment.</span></span> <span data-ttu-id="73b3a-109">Tento postup je vhodnější spuštění aplikace vlastní oprávnění, protože:</span><span class="sxs-lookup"><span data-stu-id="73b3a-109">This approach is preferable to running the app under your own credentials because:</span></span>

* <span data-ttu-id="73b3a-110">Můžete přiřadit oprávnění k identitě aplikace, která se liší od vlastní oprávnění.</span><span class="sxs-lookup"><span data-stu-id="73b3a-110">You can assign permissions to the app identity that are different from your own permissions.</span></span> <span data-ttu-id="73b3a-111">Tato oprávnění jsou obvykle omezené na přesně co aplikaci je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="73b3a-111">Typically, these permissions are restricted to exactly what the app needs to do.</span></span> <span data-ttu-id="73b3a-112">Můžete například povolit aplikaci pouze číst data v prostředí s konkrétní Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="73b3a-112">For example, you can allow the app to only read data in a particular Time Series Insights environment.</span></span>
* <span data-ttu-id="73b3a-113">Nemáte ke změně pověření aplikace, pokud vaše odpovědnosti změnit.</span><span class="sxs-lookup"><span data-stu-id="73b3a-113">You don't have to change the app's credentials if your responsibilities change.</span></span>
* <span data-ttu-id="73b3a-114">Certifikát nebo klíč aplikace můžete použít k automatizaci ověřování, když používáte bezobslužného skriptu.</span><span class="sxs-lookup"><span data-stu-id="73b3a-114">You can use a certificate or an application key to automate authentication when you're running an unattended script.</span></span>

<span data-ttu-id="73b3a-115">Tento článek ukazuje, jak provádět tyto kroky prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="73b3a-115">This article shows you how to perform those steps through the Azure portal.</span></span> <span data-ttu-id="73b3a-116">Zaměřuje se na jednoho klienta aplikace, kde je záměrem aplikaci spustit v pouze jedné organizaci.</span><span class="sxs-lookup"><span data-stu-id="73b3a-116">It focuses on a single-tenant application where the application is intended to run in only one organization.</span></span> <span data-ttu-id="73b3a-117">Jednoho klienta aplikace se obvykle používají pro-obchodní aplikace, které běží ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="73b3a-117">You typically use single-tenant applications for line-of-business applications that run in your organization.</span></span>

<span data-ttu-id="73b3a-118">Nastavení toku se skládá z tři hlavní kroky:</span><span class="sxs-lookup"><span data-stu-id="73b3a-118">The setup flow consists of three high-level steps:</span></span>

1. <span data-ttu-id="73b3a-119">Vytvoření aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73b3a-119">Create an application in Azure Active Directory.</span></span>
2. <span data-ttu-id="73b3a-120">Autorizovat tuto aplikaci pro přístup k prostředí Statistika časové řady.</span><span class="sxs-lookup"><span data-stu-id="73b3a-120">Authorize this application to access the Time Series Insights environment.</span></span>
3. <span data-ttu-id="73b3a-121">Pomůže získat token pro ID aplikace a klíč `"https://api.timeseries.azure.com/"` cílové skupiny nebo prostředku.</span><span class="sxs-lookup"><span data-stu-id="73b3a-121">Use the application ID and key to acquire a token to the `"https://api.timeseries.azure.com/"` audience or resource.</span></span> <span data-ttu-id="73b3a-122">Token poté slouží k volání rozhraní API pro přehledy časové řady.</span><span class="sxs-lookup"><span data-stu-id="73b3a-122">The token can then be used to call the Time Series Insights API.</span></span>

<span data-ttu-id="73b3a-123">Tady jsou podrobné kroky:</span><span class="sxs-lookup"><span data-stu-id="73b3a-123">Here are the detailed steps:</span></span>

1. <span data-ttu-id="73b3a-124">Na portálu Azure vyberte **Azure Active Directory** > **registrace aplikace** > **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="73b3a-124">In the Azure portal, select **Azure Active Directory** > **App registrations** > **New application registration**.</span></span>

   ![Nové registrace aplikace v Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. <span data-ttu-id="73b3a-126">Zadejte název aplikace, vyberte typ, který má být **webovou aplikaci nebo API**, vyberte libovolný platný identifikátor URI pro **přihlašovací adresa URL**a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="73b3a-126">Give the application a name, select the type to be **Web app / API**, select any valid URI for **Sign-on URL**, and click **Create**.</span></span>

   ![Vytvoření aplikace v Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. <span data-ttu-id="73b3a-128">Vyberte nově vytvořené aplikace a zkopírujte jeho ID aplikace na svém oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="73b3a-128">Select your newly created application and copy its application ID to your favorite text editor.</span></span>

   ![Zkopírujte ID aplikace](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. <span data-ttu-id="73b3a-130">Vyberte **klíče**, zadejte název klíče, vyberte vypršení platnosti a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="73b3a-130">Select **Keys**, enter the key name, select the expiration, and click **Save**.</span></span>

   ![Výběr aplikace klíče](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Zadejte název klíče a vypršení platnosti a klikněte na tlačítko Uložit](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. <span data-ttu-id="73b3a-133">Zkopírujte klíč na svém oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="73b3a-133">Copy the key to your favorite text editor.</span></span>

   ![Zkopírovat klíč aplikace](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. <span data-ttu-id="73b3a-135">Pro časové řady Statistika prostředí, vyberte **zásady přístupu k datům** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="73b3a-135">For the Time Series Insights environment, select **Data Access Policies** and click **Add**.</span></span>

   ![Přidat nové zásady přístupu data do prostředí Statistika časové řady](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. <span data-ttu-id="73b3a-137">V **vyberte uživatele** dialogové okno, vložte název aplikace (z kroku 2) nebo ID aplikace (z kroku 3).</span><span class="sxs-lookup"><span data-stu-id="73b3a-137">In the **Select User** dialog box, paste the application name (from step 2) or application ID (from step 3).</span></span>

   ![Vyhledání aplikace v dialogovém okně vyberte uživatele](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. <span data-ttu-id="73b3a-139">Vyberte roli (**čtečky** pro dotazování na data, **Přispěvatel** pro dotazování dat a změna referenční data) a klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="73b3a-139">Select the role (**Reader** for querying data, **Contributor** for querying data and changing reference data) and click **Ok**.</span></span>

   ![Vyberte čtečky nebo Přispěvatel v dialogovém okně vybrat roli](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. <span data-ttu-id="73b3a-141">Uložit zásadu kliknutím **Ok**.</span><span class="sxs-lookup"><span data-stu-id="73b3a-141">Save the policy by clicking **Ok**.</span></span>

10. <span data-ttu-id="73b3a-142">Pomocí ID aplikace (z kroku 3) a klíč aplikace (z kroku 5) k získání tokenu jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="73b3a-142">Use the application ID (from step 3) and application key (from step 5) to acquire the token on behalf of the application.</span></span> <span data-ttu-id="73b3a-143">Token je pak možné předat v `Authorization` záhlaví při volání rozhraní API pro přehledy časové řady.</span><span class="sxs-lookup"><span data-stu-id="73b3a-143">The token can then be passed in the `Authorization` header when the application calls the Time Series Insights API.</span></span>

    <span data-ttu-id="73b3a-144">Pokud používáte C#, můžete použít následující kód k získání tokenu jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="73b3a-144">If you're using C#, you can use the following code to acquire the token on behalf of the application.</span></span> <span data-ttu-id="73b3a-145">Kompletní příklad, najdete v části [dotaz na data pomocí jazyka C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="73b3a-145">For a complete sample, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a><span data-ttu-id="73b3a-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73b3a-146">Next steps</span></span>

<span data-ttu-id="73b3a-147">Pomocí ID aplikace a klíč ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="73b3a-147">Use the application ID and key in your application.</span></span> <span data-ttu-id="73b3a-148">Ukázkový kód, který volá rozhraní API pro přehledy časové řady, najdete v části [dotaz na data pomocí jazyka C#](time-series-insights-query-data-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="73b3a-148">For sample code that calls the Time Series Insights API, see [Query data using C#](time-series-insights-query-data-csharp.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="73b3a-149">Viz také</span><span class="sxs-lookup"><span data-stu-id="73b3a-149">See also</span></span>

* <span data-ttu-id="73b3a-150">[Dotaz na rozhraní API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) pro úplné referenční dokumentace rozhraní API dotazu</span><span class="sxs-lookup"><span data-stu-id="73b3a-150">[Query API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) for the full Query API reference</span></span>
* [<span data-ttu-id="73b3a-151">Vytvoření instančního objektu na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="73b3a-151">Create a service principal in the Azure portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
