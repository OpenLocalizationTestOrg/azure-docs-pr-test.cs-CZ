---
title: "aaaGet spuštěné s ověřováním Azure AD pomocí portálu Azure hello | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure portálu tooaccess tooconsume ověřování Azure Active Directory (Azure AD) hello rozhraní API služby Azure Media Services."
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
ms.openlocfilehash: 497ad1806b3fd1262802adefb6e12b65ee9f2d61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-hello-azure-portal"></a><span data-ttu-id="4968f-103">Začínáme s ověřováním Azure AD pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="4968f-103">Get started with Azure AD authentication by using hello Azure portal</span></span>

<span data-ttu-id="4968f-104">Zjistěte, jak toouse hello Azure tooaccess portálu služby Azure Active Directory (Azure AD) ověřování tooaccess hello rozhraní API služby Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="4968f-104">Learn how toouse hello Azure portal tooaccess Azure Active Directory (Azure AD) authentication tooaccess hello Azure Media Services API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4968f-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4968f-105">Prerequisites</span></span>

- <span data-ttu-id="4968f-106">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="4968f-106">An Azure account.</span></span> <span data-ttu-id="4968f-107">Pokud nemáte účet, začínat [bezplatné zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4968f-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="4968f-108">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="4968f-108">A Media Services account.</span></span> <span data-ttu-id="4968f-109">Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí portálu Azure hello](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="4968f-109">For more information, see [Create an Azure Media Services account by using hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="4968f-110">Ujistěte se, zkontrolujte hello [přístup k Azure Media Services API s Azure AD authentication přehled](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="4968f-110">Make sure you review hello [Accessing Azure Media Services API with Azure AD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="4968f-111">Při použití ověřování Azure AD pomocí služby Azure Media Services, máte dvě možnosti ověřování:</span><span class="sxs-lookup"><span data-stu-id="4968f-111">When you use Azure AD authentication with Azure Media Services, you have two authentication options:</span></span>

- <span data-ttu-id="4968f-112">**Ověření uživatele**.</span><span class="sxs-lookup"><span data-stu-id="4968f-112">**User authentication**.</span></span> <span data-ttu-id="4968f-113">Ověření osoba, která používá toointeract aplikace hello s prostředky Media Services.</span><span class="sxs-lookup"><span data-stu-id="4968f-113">Authenticate a person who is using hello app toointeract with Media Services resources.</span></span> <span data-ttu-id="4968f-114">interaktivní aplikace Hello měli nejdřív hello uživatele vyzve k přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4968f-114">hello interactive application should first prompt hello user for credentials.</span></span> <span data-ttu-id="4968f-115">Příklad je používán oprávněným uživatelům toomonitor kódování úlohy správy konzoly aplikace nebo živé streamování.</span><span class="sxs-lookup"><span data-stu-id="4968f-115">An example is a management console app used by authorized users toomonitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="4968f-116">**Objekt zabezpečení ověřování služby**.</span><span class="sxs-lookup"><span data-stu-id="4968f-116">**Service principal authentication**.</span></span> <span data-ttu-id="4968f-117">Ověření služby.</span><span class="sxs-lookup"><span data-stu-id="4968f-117">Authenticate a service.</span></span> <span data-ttu-id="4968f-118">Aplikace, které běžně používají tuto metodu ověřování, jsou aplikace, které běží služby démon, střední vrstvy služby nebo naplánované úlohy: webové aplikace, funkce aplikace, aplikace logiky, rozhraní API nebo mikroslužby.</span><span class="sxs-lookup"><span data-stu-id="4968f-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs: web apps, function apps, logic apps, APIs, or a microservice.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4968f-119">V současné době Media Services podporuje model ověřování služby Řízení přístupu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="4968f-119">Currently, Media Services supports hello Azure Access Control service authentication model.</span></span> <span data-ttu-id="4968f-120">Řízení přístupu autorizace však bude na 1. června 2018 zastaralá.</span><span class="sxs-lookup"><span data-stu-id="4968f-120">However, Access Control authorization will be deprecated on June 1, 2018.</span></span> <span data-ttu-id="4968f-121">Doporučujeme, abyste co nejdříve migraci model ověřování toohello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4968f-121">We recommend that you migrate toohello Azure AD authentication model as soon as possible.</span></span>

## <a name="select-hello-authentication-method"></a><span data-ttu-id="4968f-122">Vyberte metodu ověřování hello</span><span class="sxs-lookup"><span data-stu-id="4968f-122">Select hello authentication method</span></span>

1. <span data-ttu-id="4968f-123">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="4968f-123">In hello [Azure portal](https://portal.azure.com/), select your Media Services account.</span></span>
2. <span data-ttu-id="4968f-124">Vyberte, jak tooconnect toohello Media Services API.</span><span class="sxs-lookup"><span data-stu-id="4968f-124">Select how tooconnect toohello Media Services API.</span></span>

    ![Vyberte stránku metoda připojení](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a><span data-ttu-id="4968f-126">Ověřování uživatelů</span><span class="sxs-lookup"><span data-stu-id="4968f-126">User authentication</span></span>

<span data-ttu-id="4968f-127">tooconnect toohello Media Services API pomocí hello možnost ověřování uživatelů, klientská aplikace hello musí toorequest tokenu Azure AD, který má hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="4968f-127">tooconnect toohello Media Services API by using hello user authentication option, hello client app needs toorequest an Azure AD token that has hello following parameters:</span></span>  

* <span data-ttu-id="4968f-128">Koncový bod tenant Azure AD</span><span class="sxs-lookup"><span data-stu-id="4968f-128">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="4968f-129">Identifikátor URI prostředku Media Services</span><span class="sxs-lookup"><span data-stu-id="4968f-129">Media Services resource URI</span></span>
* <span data-ttu-id="4968f-130">ID klienta aplikace Media Services (nativní)</span><span class="sxs-lookup"><span data-stu-id="4968f-130">Media Services (native) application client ID</span></span> 
* <span data-ttu-id="4968f-131">Identifikátor URI pro přesměrování aplikace Media Services (nativní)</span><span class="sxs-lookup"><span data-stu-id="4968f-131">Media Services (native) application redirect URI</span></span> 
* <span data-ttu-id="4968f-132">Identifikátor URI pro REST Media Services</span><span class="sxs-lookup"><span data-stu-id="4968f-132">Resource URI for REST Media Services</span></span>

<span data-ttu-id="4968f-133">Můžete získat hello hodnoty pro tyto parametry hello **Media Services API s ověřováním uživatele** stránky.</span><span class="sxs-lookup"><span data-stu-id="4968f-133">You can get hello values for these parameters on hello **Media Services API with user authentication** page.</span></span> 

![Připojení k stránce ověřování uživatele](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

<span data-ttu-id="4968f-135">Pokud připojíte toohello Media Services API pomocí hello Media Services Microsoft .NET SDK, hello požadované hodnoty jsou k dispozici tooyou jako součást hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4968f-135">If you connect toohello Media Services API by using hello Media Services Microsoft .NET SDK, hello required values are available tooyou as part of hello SDK.</span></span> <span data-ttu-id="4968f-136">Další informace najdete v tématu [používání Azure AD authentication tooaccess hello rozhraní API služby Azure Media Services s .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="4968f-136">For more information, see [Use Azure AD authentication tooaccess hello Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="4968f-137">Pokud nepoužíváte klienta hello Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu Azure AD pomocí parametrů hello uvedenými výše.</span><span class="sxs-lookup"><span data-stu-id="4968f-137">If you're not using hello Media Services .NET client SDK, you must manually create an Azure AD token request by using hello parameters discussed earlier.</span></span> <span data-ttu-id="4968f-138">Další informace najdete v tématu [jak toouse hello Azure AD Authentication Library tooget hello tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="4968f-138">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

## <a name="service-principal-authentication"></a><span data-ttu-id="4968f-139">Ověřování instančních objektů</span><span class="sxs-lookup"><span data-stu-id="4968f-139">Service principal authentication</span></span>

<span data-ttu-id="4968f-140">tooconnect toohello Media Services API pomocí hello hlavní možnosti služby, vaše aplikace střední vrstvy (webového rozhraní API nebo webové aplikace) musí toorequest tokenu Azure AD, který má hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="4968f-140">tooconnect toohello Media Services API by using hello service principal option, your middle-tier app (web API or web application) needs toorequest an Azure AD token that has hello following parameters:</span></span>  

* <span data-ttu-id="4968f-141">Koncový bod tenant Azure AD</span><span class="sxs-lookup"><span data-stu-id="4968f-141">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="4968f-142">Identifikátor URI prostředku Media Services</span><span class="sxs-lookup"><span data-stu-id="4968f-142">Media Services resource URI</span></span> 
* <span data-ttu-id="4968f-143">Identifikátor URI pro REST Media Services</span><span class="sxs-lookup"><span data-stu-id="4968f-143">Resource URI for REST Media Services</span></span>
* <span data-ttu-id="4968f-144">Hodnoty aplikace Azure AD: hello **ID klienta** a **tajný klíč klienta**</span><span class="sxs-lookup"><span data-stu-id="4968f-144">Azure AD application values: hello **client ID** and **client secret**</span></span>

<span data-ttu-id="4968f-145">Můžete získat hello hodnoty pro tyto parametry hello **tooMedia rozhraní API služby připojit pomocí objektu služby** stránky.</span><span class="sxs-lookup"><span data-stu-id="4968f-145">You can get hello values for these parameters on hello **Connect tooMedia Services API with service principal** page.</span></span> <span data-ttu-id="4968f-146">Pomocí této stránky toocreate nové Azure aplikaci AD nebo tooselect některého ze stávajících.</span><span class="sxs-lookup"><span data-stu-id="4968f-146">Use this page toocreate a new Azure AD application or tooselect an existing one.</span></span> <span data-ttu-id="4968f-147">Po výběru hello aplikaci Azure AD, můžete získat hello ID klienta (ID aplikace) a generovat hello klienta tajný klíč (klíč) hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4968f-147">After you select hello Azure AD app, you can get hello client ID (Application ID) and generate hello client secret (key) values.</span></span> 

![Připojení k hlavní stránce služby](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

<span data-ttu-id="4968f-149">Když hello **instanční objekt** otevře se okno, je vybrána hello první aplikaci Azure AD splňující hello následující kritéria:</span><span class="sxs-lookup"><span data-stu-id="4968f-149">When hello **Service Principal** blade opens, hello first Azure AD application that meets hello following criteria is selected:</span></span>

- <span data-ttu-id="4968f-150">Je registrovaný aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4968f-150">It is a registered Azure AD application.</span></span>
- <span data-ttu-id="4968f-151">Na hello účet má oprávnění přispěvatele nebo Owner Role-Based řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="4968f-151">It has Contributor or Owner Role-Based Access Control permissions on hello account.</span></span>

<span data-ttu-id="4968f-152">Po vytvoření nebo vyberte aplikaci Azure AD, můžete vytvořit a zkopírovat tajný klíč klienta (klíč) a hello ID klienta (ID aplikace).</span><span class="sxs-lookup"><span data-stu-id="4968f-152">After you create or select an Azure AD app, you can create and copy a client secret (key) and hello client ID (Application ID).</span></span> <span data-ttu-id="4968f-153">ID klienta tajný klíč a klienta Hello jsou požadované tooget hello přístupový token v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="4968f-153">hello client secret and client ID are required tooget hello access token in this scenario.</span></span>

<span data-ttu-id="4968f-154">Pokud nemáte oprávnění aplikace Azure AD toocreate ve vaší doméně, nejsou zobrazeny ovládací prvky aplikace Azure AD hello hello okně a zobrazí zpráva s upozorněním.</span><span class="sxs-lookup"><span data-stu-id="4968f-154">If you don't have permissions toocreate Azure AD apps in your domain, hello Azure AD app controls of hello blade are not shown, and a warning message is displayed.</span></span>

<span data-ttu-id="4968f-155">Pokud připojíte toohello Media Services API pomocí hello sady Media Services .NET SDK, přečtěte si téma [používání Azure AD authentication tooaccess hello rozhraní API služby Azure Media Services s .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="4968f-155">If you connect toohello Media Services API by using hello Media Services .NET SDK, see [Use Azure AD authentication tooaccess hello Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="4968f-156">Pokud nepoužíváte klienta hello Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu Azure AD pomocí parametrů hello uvedenými výše.</span><span class="sxs-lookup"><span data-stu-id="4968f-156">If you are not using hello Media Services .NET client SDK, you must manually create an Azure AD token request using hello parameters discussed earlier.</span></span> <span data-ttu-id="4968f-157">Další informace najdete v tématu [jak toouse hello Azure AD Authentication Library tooget hello tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="4968f-157">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

### <a name="get-hello-client-id-and-client-secret"></a><span data-ttu-id="4968f-158">Získat ID a klienta sdílený tajný klíč klienta hello</span><span class="sxs-lookup"><span data-stu-id="4968f-158">Get hello client ID and client secret</span></span>

<span data-ttu-id="4968f-159">Po výběru existující aplikace Azure AD nebo toocreate hello vyberte možnost Nový, objeví se hello následující tlačítka:</span><span class="sxs-lookup"><span data-stu-id="4968f-159">After you select an existing Azure AD app or select hello option toocreate a new one, hello following buttons appear:</span></span>

![Správa oprávnění tlačítka a Správa aplikací](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

<span data-ttu-id="4968f-161">Klikněte na tlačítko tooopen hello okna aplikací Azure AD, **správě aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4968f-161">tooopen hello Azure AD application blade, click **Manage application**.</span></span> <span data-ttu-id="4968f-162">Na hello **správě aplikace** okně můžete získat ID klienta aplikace hello (ID aplikace).</span><span class="sxs-lookup"><span data-stu-id="4968f-162">On hello **Manage application** blade, you can get hello app's client ID (Application ID).</span></span> <span data-ttu-id="4968f-163">toogenerate tajný klíč klienta (klíč), vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="4968f-163">toogenerate a client secret (key), select **Keys**.</span></span>

![Spravovat aplikace okna klíče možnost](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-hello-application"></a><span data-ttu-id="4968f-165">Správa oprávnění a aplikace hello</span><span class="sxs-lookup"><span data-stu-id="4968f-165">Manage permissions and hello application</span></span>

<span data-ttu-id="4968f-166">Jakmile vyberete aplikace hello Azure AD, můžete spravovat aplikace hello a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4968f-166">After you select hello Azure AD application, you can manage hello application and permissions.</span></span> <span data-ttu-id="4968f-167">tooset si vaše tooaccess aplikace Azure AD jiné aplikace, klikněte na **spravovat oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="4968f-167">tooset up your Azure AD application tooaccess other applications, click **Manage permissions**.</span></span> <span data-ttu-id="4968f-168">Pro úlohy správy, jako je například změna klíčů a adresy URL odpovědí nebo manifest aplikace hello tooedit, klikněte na tlačítko **správě aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4968f-168">For management tasks, such as changing keys and reply URLs, or tooedit hello application’s manifest, click **Manage application**.</span></span>

### <a name="edit-hello-apps-settings-or-manifest"></a><span data-ttu-id="4968f-169">Upravit nastavení aplikace hello nebo manifest</span><span class="sxs-lookup"><span data-stu-id="4968f-169">Edit hello app's settings or manifest</span></span>

<span data-ttu-id="4968f-170">nastavení aplikace hello tooedit nebo manifest, klikněte na tlačítko **správě aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4968f-170">tooedit hello app's settings or manifest, click **Manage application**.</span></span>

![Spravovat stránky aplikace](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a><span data-ttu-id="4968f-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4968f-172">Next steps</span></span>

<span data-ttu-id="4968f-173">Začínáme s [nahrávání souborů tooyour účet](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="4968f-173">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
