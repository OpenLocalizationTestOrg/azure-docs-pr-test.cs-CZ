---
title: "Začínáme s ověřováním Azure AD pomocí portálu Azure | Microsoft Docs"
description: "Naučte se používat portál Azure pro přístup k ověřování Azure Active Directory (Azure AD) využívají rozhraní API služby Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 829e0759f8aeb8758f6b8895b88b60b66ffb22ed
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a><span data-ttu-id="e1874-103">Začínáme s ověřováním Azure AD pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e1874-103">Get started with Azure AD authentication by using the Azure portal</span></span>

<span data-ttu-id="e1874-104">Naučte se používat portál Azure pro přístup k ověřování Azure Active Directory (Azure AD) pro přístup k rozhraní API služby Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="e1874-104">Learn how to use the Azure portal to access Azure Active Directory (Azure AD) authentication to access the Azure Media Services API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1874-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1874-105">Prerequisites</span></span>

- <span data-ttu-id="e1874-106">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="e1874-106">An Azure account.</span></span> <span data-ttu-id="e1874-107">Pokud nemáte účet, začínat [bezplatné zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e1874-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="e1874-108">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="e1874-108">A Media Services account.</span></span> <span data-ttu-id="e1874-109">Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí portálu Azure](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="e1874-109">For more information, see [Create an Azure Media Services account by using the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="e1874-110">Ujistěte se, můžete zkontrolovat [přístup k Azure Media Services API s Azure AD authentication přehled](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="e1874-110">Make sure you review the [Accessing Azure Media Services API with Azure AD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="e1874-111">Při použití ověřování Azure AD pomocí služby Azure Media Services, máte dvě možnosti ověřování:</span><span class="sxs-lookup"><span data-stu-id="e1874-111">When you use Azure AD authentication with Azure Media Services, you have two authentication options:</span></span>

- <span data-ttu-id="e1874-112">**Ověření uživatele**.</span><span class="sxs-lookup"><span data-stu-id="e1874-112">**User authentication**.</span></span> <span data-ttu-id="e1874-113">Ověření osoba, která používá aplikace komunikovat s prostředky Media Services.</span><span class="sxs-lookup"><span data-stu-id="e1874-113">Authenticate a person who is using the app to interact with Media Services resources.</span></span> <span data-ttu-id="e1874-114">Interaktivní aplikace by měla nejprve vyzvou uživatele k zadání přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e1874-114">The interactive application should first prompt the user for credentials.</span></span> <span data-ttu-id="e1874-115">Příkladem je aplikace konzoly správy Autorizovaní uživatelé používají ke sledování úloh kódování nebo živé streamování.</span><span class="sxs-lookup"><span data-stu-id="e1874-115">An example is a management console app used by authorized users to monitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="e1874-116">**Objekt zabezpečení ověřování služby**.</span><span class="sxs-lookup"><span data-stu-id="e1874-116">**Service principal authentication**.</span></span> <span data-ttu-id="e1874-117">Ověření služby.</span><span class="sxs-lookup"><span data-stu-id="e1874-117">Authenticate a service.</span></span> <span data-ttu-id="e1874-118">Aplikace, které běžně používají tuto metodu ověřování, jsou aplikace, které běží služby démon, střední vrstvy služby nebo naplánované úlohy: webové aplikace, funkce aplikace, aplikace logiky, rozhraní API nebo mikroslužby.</span><span class="sxs-lookup"><span data-stu-id="e1874-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs: web apps, function apps, logic apps, APIs, or a microservice.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e1874-119">V současné době Media Services podporuje model řízení přístupu Azure služby ověřování.</span><span class="sxs-lookup"><span data-stu-id="e1874-119">Currently, Media Services supports the Azure Access Control service authentication model.</span></span> <span data-ttu-id="e1874-120">Řízení přístupu autorizace však bude na 1. června 2018 zastaralá.</span><span class="sxs-lookup"><span data-stu-id="e1874-120">However, Access Control authorization will be deprecated on June 1, 2018.</span></span> <span data-ttu-id="e1874-121">Doporučujeme, abyste přenést do Azure AD authentication modelu co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="e1874-121">We recommend that you migrate to the Azure AD authentication model as soon as possible.</span></span>

## <a name="select-the-authentication-method"></a><span data-ttu-id="e1874-122">Vyberte metodu ověřování</span><span class="sxs-lookup"><span data-stu-id="e1874-122">Select the authentication method</span></span>

1. <span data-ttu-id="e1874-123">V [portál Azure](https://portal.azure.com/), vyberte svůj účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="e1874-123">In the [Azure portal](https://portal.azure.com/), select your Media Services account.</span></span>
2. <span data-ttu-id="e1874-124">Vyberte, jak se připojit k rozhraní API služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="e1874-124">Select how to connect to the Media Services API.</span></span>

    ![Vyberte stránku metoda připojení](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a><span data-ttu-id="e1874-126">Ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e1874-126">User authentication</span></span>

<span data-ttu-id="e1874-127">Pro připojení k rozhraní API služby Media Services pomocí možnosti ověřování uživatele, klientská aplikace potřebuje k vyžádání tokenu Azure AD, který má následující parametry:</span><span class="sxs-lookup"><span data-stu-id="e1874-127">To connect to the Media Services API by using the user authentication option, the client app needs to request an Azure AD token that has the following parameters:</span></span>  

* <span data-ttu-id="e1874-128">Koncový bod tenant Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1874-128">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="e1874-129">Identifikátor URI prostředku Media Services</span><span class="sxs-lookup"><span data-stu-id="e1874-129">Media Services resource URI</span></span>
* <span data-ttu-id="e1874-130">ID klienta aplikace Media Services (nativní)</span><span class="sxs-lookup"><span data-stu-id="e1874-130">Media Services (native) application client ID</span></span> 
* <span data-ttu-id="e1874-131">Identifikátor URI pro přesměrování aplikace Media Services (nativní)</span><span class="sxs-lookup"><span data-stu-id="e1874-131">Media Services (native) application redirect URI</span></span> 
* <span data-ttu-id="e1874-132">Identifikátor URI pro REST Media Services</span><span class="sxs-lookup"><span data-stu-id="e1874-132">Resource URI for REST Media Services</span></span>

<span data-ttu-id="e1874-133">Hodnoty pro tyto parametry můžete získat **Media Services API s ověřováním uživatele** stránky.</span><span class="sxs-lookup"><span data-stu-id="e1874-133">You can get the values for these parameters on the **Media Services API with user authentication** page.</span></span> 

![Připojení k stránce ověřování uživatele](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

<span data-ttu-id="e1874-135">Pokud se připojit k rozhraní API služby Media Services pomocí .NET SDK služby Media Services Microsoft požadované hodnoty jsou k dispozici jako součást sady SDK.</span><span class="sxs-lookup"><span data-stu-id="e1874-135">If you connect to the Media Services API by using the Media Services Microsoft .NET SDK, the required values are available to you as part of the SDK.</span></span> <span data-ttu-id="e1874-136">Další informace najdete v tématu [ověřování pomocí služby Azure AD pro přístup k rozhraní API Azure Media Services pomocí rozhraní .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="e1874-136">For more information, see [Use Azure AD authentication to access the Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="e1874-137">Pokud nepoužíváte klienta Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu Azure AD pomocí parametrů už jsme probírali výše.</span><span class="sxs-lookup"><span data-stu-id="e1874-137">If you're not using the Media Services .NET client SDK, you must manually create an Azure AD token request by using the parameters discussed earlier.</span></span> <span data-ttu-id="e1874-138">Další informace najdete v tématu [jak používat knihovnu ověřování služby Azure AD k získání tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="e1874-138">For more information, see [How to use the Azure AD Authentication Library to get the Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

## <a name="service-principal-authentication"></a><span data-ttu-id="e1874-139">Ověřování instančních objektů</span><span class="sxs-lookup"><span data-stu-id="e1874-139">Service principal authentication</span></span>

<span data-ttu-id="e1874-140">Vaše aplikace střední vrstvy (webového rozhraní API nebo webové aplikace) pro připojení k rozhraní API služby Media Services pomocí možnosti hlavní služby, musí žádosti o token Azure AD, který má následující parametry:</span><span class="sxs-lookup"><span data-stu-id="e1874-140">To connect to the Media Services API by using the service principal option, your middle-tier app (web API or web application) needs to request an Azure AD token that has the following parameters:</span></span>  

* <span data-ttu-id="e1874-141">Koncový bod tenant Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1874-141">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="e1874-142">Identifikátor URI prostředku Media Services</span><span class="sxs-lookup"><span data-stu-id="e1874-142">Media Services resource URI</span></span> 
* <span data-ttu-id="e1874-143">Identifikátor URI pro REST Media Services</span><span class="sxs-lookup"><span data-stu-id="e1874-143">Resource URI for REST Media Services</span></span>
* <span data-ttu-id="e1874-144">Hodnoty aplikace Azure AD: **ID klienta** a **tajný klíč klienta**</span><span class="sxs-lookup"><span data-stu-id="e1874-144">Azure AD application values: the **client ID** and **client secret**</span></span>

<span data-ttu-id="e1874-145">Hodnoty pro tyto parametry můžete získat **připojit k rozhraní API služby média pomocí objektu služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="e1874-145">You can get the values for these parameters on the **Connect to Media Services API with service principal** page.</span></span> <span data-ttu-id="e1874-146">Na této stránce můžete vytvořit novou aplikaci Azure AD nebo vybrat stávající.</span><span class="sxs-lookup"><span data-stu-id="e1874-146">Use this page to create a new Azure AD application or to select an existing one.</span></span> <span data-ttu-id="e1874-147">Když vyberete aplikaci Azure AD, můžete získat ID klienta (ID aplikace) a generování hodnot tajný klíč (klíč) klienta.</span><span class="sxs-lookup"><span data-stu-id="e1874-147">After you select the Azure AD app, you can get the client ID (Application ID) and generate the client secret (key) values.</span></span> 

![Připojení k hlavní stránce služby](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

<span data-ttu-id="e1874-149">Když **instanční objekt** otevře se okno, je vybrána první aplikace Azure AD, která splňuje následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="e1874-149">When the **Service Principal** blade opens, the first Azure AD application that meets the following criteria is selected:</span></span>

- <span data-ttu-id="e1874-150">Je registrovaný aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e1874-150">It is a registered Azure AD application.</span></span>
- <span data-ttu-id="e1874-151">Na účet má oprávnění přispěvatele nebo Owner Role-Based řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="e1874-151">It has Contributor or Owner Role-Based Access Control permissions on the account.</span></span>

<span data-ttu-id="e1874-152">Po vytvoření nebo vyberte aplikaci Azure AD, můžete vytvořit a zkopírovat tajný klíč klienta (klíč) a ID klienta (ID aplikace).</span><span class="sxs-lookup"><span data-stu-id="e1874-152">After you create or select an Azure AD app, you can create and copy a client secret (key) and the client ID (Application ID).</span></span> <span data-ttu-id="e1874-153">K získání přístupu tokenu v tomto scénáři se vyžadují tajný klíč klienta a ID klienta.</span><span class="sxs-lookup"><span data-stu-id="e1874-153">The client secret and client ID are required to get the access token in this scenario.</span></span>

<span data-ttu-id="e1874-154">Pokud nemáte oprávnění k vytvoření aplikace Azure AD ve vaší doméně, nejsou zobrazeny v okně Ovládací prvky aplikace Azure AD a zobrazí zpráva s upozorněním.</span><span class="sxs-lookup"><span data-stu-id="e1874-154">If you don't have permissions to create Azure AD apps in your domain, the Azure AD app controls of the blade are not shown, and a warning message is displayed.</span></span>

<span data-ttu-id="e1874-155">Pokud se připojit k rozhraní API služby Media Services pomocí sady Media Services .NET SDK najdete v části [ověřování pomocí služby Azure AD pro přístup k rozhraní API Azure Media Services pomocí rozhraní .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="e1874-155">If you connect to the Media Services API by using the Media Services .NET SDK, see [Use Azure AD authentication to access the Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="e1874-156">Pokud nepoužíváte klienta Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu Azure AD pomocí parametrů už jsme probírali výše.</span><span class="sxs-lookup"><span data-stu-id="e1874-156">If you are not using the Media Services .NET client SDK, you must manually create an Azure AD token request using the parameters discussed earlier.</span></span> <span data-ttu-id="e1874-157">Další informace najdete v tématu [jak používat knihovnu ověřování služby Azure AD k získání tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="e1874-157">For more information, see [How to use the Azure AD Authentication Library to get the Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

### <a name="get-the-client-id-and-client-secret"></a><span data-ttu-id="e1874-158">Získání ID klienta a tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="e1874-158">Get the client ID and client secret</span></span>

<span data-ttu-id="e1874-159">Jakmile vybrat existující aplikaci Azure AD nebo vybrat možnost vytvořit novou, se zobrazí následující tlačítka:</span><span class="sxs-lookup"><span data-stu-id="e1874-159">After you select an existing Azure AD app or select the option to create a new one, the following buttons appear:</span></span>

![Správa oprávnění tlačítka a Správa aplikací](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

<span data-ttu-id="e1874-161">Chcete-li otevřít okno aplikace Azure AD, klikněte na tlačítko **správě aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e1874-161">To open the Azure AD application blade, click **Manage application**.</span></span> <span data-ttu-id="e1874-162">Na **správě aplikace** okně můžete získat ID klienta aplikace (ID aplikace).</span><span class="sxs-lookup"><span data-stu-id="e1874-162">On the **Manage application** blade, you can get the app's client ID (Application ID).</span></span> <span data-ttu-id="e1874-163">Chcete-li vygenerovat tajný klíč klienta (klíč), vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="e1874-163">To generate a client secret (key), select **Keys**.</span></span>

![Spravovat aplikace okna klíče možnost](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a><span data-ttu-id="e1874-165">Správa oprávnění a aplikace</span><span class="sxs-lookup"><span data-stu-id="e1874-165">Manage permissions and the application</span></span>

<span data-ttu-id="e1874-166">Jakmile vyberete aplikace Azure AD, můžete spravovat aplikace a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e1874-166">After you select the Azure AD application, you can manage the application and permissions.</span></span> <span data-ttu-id="e1874-167">Chcete-li nastavit aplikaci Azure AD pro přístup k jiné aplikace, klikněte na tlačítko **spravovat oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="e1874-167">To set up your Azure AD application to access other applications, click **Manage permissions**.</span></span> <span data-ttu-id="e1874-168">Pro úlohy správy, jako je například změna klíčů a adresy URL odpovědí, nebo upravit manifestu aplikace, klikněte na tlačítko **správě aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e1874-168">For management tasks, such as changing keys and reply URLs, or to edit the application’s manifest, click **Manage application**.</span></span>

### <a name="edit-the-apps-settings-or-manifest"></a><span data-ttu-id="e1874-169">Upravit nastavení aplikace nebo manifest</span><span class="sxs-lookup"><span data-stu-id="e1874-169">Edit the app's settings or manifest</span></span>

<span data-ttu-id="e1874-170">Chcete-li upravit nastavení nebo manifestu aplikace, klikněte na tlačítko **správě aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e1874-170">To edit the app's settings or manifest, click **Manage application**.</span></span>

![Spravovat stránky aplikace](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a><span data-ttu-id="e1874-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1874-172">Next steps</span></span>

<span data-ttu-id="e1874-173">Začínáme s [nahrávání souborů do účtu](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="e1874-173">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
