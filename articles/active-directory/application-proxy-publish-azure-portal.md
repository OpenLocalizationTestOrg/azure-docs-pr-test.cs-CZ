---
title: "Publikování aplikací pomocí proxy aplikace služby Azure AD | Dokumentace Microsoftu"
description: "Publikujte místní aplikace do cloudu s Azure AD Application Proxy na portálu Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: e00a939f2b20ab8e0a2ddf0ff91e59db440213ac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="7619c-103">Publikování aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7619c-103">Publish applications using Azure AD Application Proxy</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7619c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7619c-104">Azure portal</span></span>](application-proxy-publish-azure-portal.md)
> * [<span data-ttu-id="7619c-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="7619c-105">Azure classic portal</span></span>](active-directory-application-proxy-publish.md)

<span data-ttu-id="7619c-106">Proxy aplikace služby Azure Active Directory (AD) umožňuje podporují zaměstnanci na vzdálených pracovištích a publikování aplikací místní přístup přes internet.</span><span class="sxs-lookup"><span data-stu-id="7619c-106">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications to be accessed over the internet.</span></span> <span data-ttu-id="7619c-107">Můžete publikovat tyto aplikace prostřednictvím portálu Azure poskytnout zabezpečený vzdálený přístup z mimo vaši síť.</span><span class="sxs-lookup"><span data-stu-id="7619c-107">You can publish these applications through the Azure portal to provide secure remote access from outside your network.</span></span>

<span data-ttu-id="7619c-108">Tento článek vás provede kroky k publikování aplikace místně pomocí Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="7619c-108">This article walks you through the steps to publish an on-premises app with Application Proxy.</span></span> <span data-ttu-id="7619c-109">Po dokončení tohoto článku, bude mít přístup k aplikaci vzdáleně vaši uživatelé.</span><span class="sxs-lookup"><span data-stu-id="7619c-109">After you complete this article, your users will be able to access your app remotely.</span></span> <span data-ttu-id="7619c-110">A budete moci konfigurovat další funkce pro aplikaci jako jednotné přihlašování, přizpůsobené informace a požadavky na zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7619c-110">And you'll be ready to configure extra features for the application like single sign-on, personalized information, and security requirements.</span></span>

<span data-ttu-id="7619c-111">Pokud jste ještě Proxy aplikace, další informace o této funkci s článkem [jak poskytnout zabezpečený vzdálený přístup k místním aplikacím](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7619c-111">If you're new to Application Proxy, learn more about this feature with the article [How to provide secure remote access to on-premises applications](active-directory-application-proxy-get-started.md).</span></span>


## <a name="publish-an-on-premises-app-for-remote-access"></a><span data-ttu-id="7619c-112">Publikování aplikace místně pro vzdálený přístup</span><span class="sxs-lookup"><span data-stu-id="7619c-112">Publish an on-premises app for remote access</span></span>

<span data-ttu-id="7619c-113">Postupujte podle těchto kroků k publikování aplikací pomocí Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="7619c-113">Follow these steps to publish your apps with Application Proxy.</span></span> <span data-ttu-id="7619c-114">Pokud už nejsou stáhnout a nakonfigurovat konektor pro vaši organizaci, přejděte na [začít pracovat s Proxy aplikace a nainstalujte konektor](active-directory-application-proxy-enable.md) první a pak publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="7619c-114">If you haven't already downloaded and configured a connector for your organization, go to [Get started with Application Proxy and install the connector](active-directory-application-proxy-enable.md) first, and then publish your app.</span></span>

> [!TIP]
> <span data-ttu-id="7619c-115">Pokud testujete Proxy aplikace poprvé, vyberte aplikaci, která je nastavena pro ověřování pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="7619c-115">If you're testing out Application Proxy for the first time, choose an application that's set up for password-based authentication.</span></span> <span data-ttu-id="7619c-116">Proxy aplikací podporuje další typy ověřování, ale jsou založené na heslech aplikace nejjednodušší ke zprovoznění rychle a spustit.</span><span class="sxs-lookup"><span data-stu-id="7619c-116">Application Proxy supports other types of authentication, but password-based apps are the easiest to get up and running quickly.</span></span> 

1. <span data-ttu-id="7619c-117">Přihlaste se jako správce v [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7619c-117">Sign in as an administrator in the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="7619c-118">Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="7619c-118">Select **Azure Active Directory** > **Enterprise applications** > **New application**.</span></span>

  ![Přidat podniková aplikace](./media/application-proxy-publish-azure-portal/add-app.png)

3. <span data-ttu-id="7619c-120">Vyberte **všechny**, pak vyberte **místní aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7619c-120">Select **All**, then select **On-premises application**.</span></span>  

  ![Přidat vlastní aplikaci](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. <span data-ttu-id="7619c-122">Zadejte následující informace o vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="7619c-122">Provide the following information about your application:</span></span>

   - <span data-ttu-id="7619c-123">**Název**: název aplikace, která se zobrazí na panelu přístup a na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7619c-123">**Name**: The name of the application that will appear on the access panel and in the Azure portal.</span></span> 

   - <span data-ttu-id="7619c-124">**Interní adresa URL**: adresa URL, který používáte pro přístup k aplikaci uvnitř vaší privátní sítě.</span><span class="sxs-lookup"><span data-stu-id="7619c-124">**Internal URL**: The URL that you use to access the application from inside your private network.</span></span> <span data-ttu-id="7619c-125">Můžete zadat konkrétní cestu na beck-endovém serveru, kterou chcete publikovat, zatímco zbytek serveru publikovaný nebude.</span><span class="sxs-lookup"><span data-stu-id="7619c-125">You can provide a specific path on the backend server to publish, while the rest of the server is unpublished.</span></span> <span data-ttu-id="7619c-126">Tímto způsobem můžete publikovat různé stránky na stejném serveru jako různé aplikace a dejte každé z nich vlastní název a pravidla přístupu.</span><span class="sxs-lookup"><span data-stu-id="7619c-126">In this way, you can publish different sites on the same server as different apps, and give each one its own name and access rules.</span></span>

     > [!TIP]
     > <span data-ttu-id="7619c-127">Pokud publikujete cestu, ujistěte se, že zahrnuje všechny nezbytné obrázky, skripty a šablony stylů pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7619c-127">If you publish a path, make sure that it includes all the necessary images, scripts, and style sheets for your application.</span></span> <span data-ttu-id="7619c-128">Pokud například vaše aplikace je v cestě https://yourapp/app a používá obrázky nacházející se v cestě https://yourapp/media, pak byste měli publikujete https://yourapp/ jako cestu.</span><span class="sxs-lookup"><span data-stu-id="7619c-128">For example, if your app is at https://yourapp/app and uses images located at https://yourapp/media, then you should publish https://yourapp/ as the path.</span></span> <span data-ttu-id="7619c-129">Tato interní adresa URL nemusí být cílová stránka, která se uživatelům zobrazí.</span><span class="sxs-lookup"><span data-stu-id="7619c-129">This internal URL doesn't have to be the landing page your users see.</span></span> <span data-ttu-id="7619c-130">Další informace najdete v tématu [nastavit vlastní domovskou stránku pro publikované aplikace](application-proxy-office365-app-launcher.md).</span><span class="sxs-lookup"><span data-stu-id="7619c-130">For more information, see [Set a custom home page for published apps](application-proxy-office365-app-launcher.md).</span></span>

   - <span data-ttu-id="7619c-131">**Externí adresu URL**: adresu vaši uživatelé budou moct pro přístup k aplikaci mimo vaši síť.</span><span class="sxs-lookup"><span data-stu-id="7619c-131">**External URL**: The address your users will go to in order to access the app from outside your network.</span></span> <span data-ttu-id="7619c-132">Pokud nechcete použít výchozí doménu Proxy aplikací, přečtěte si informace o [vlastních domén v Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).</span><span class="sxs-lookup"><span data-stu-id="7619c-132">If you don't want to use the default Application Proxy domain, read about [custom domains in Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).</span></span>
   - <span data-ttu-id="7619c-133">**Předběžné ověřování**: jak Proxy aplikace ověřuje uživatele, než jim poskytne přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7619c-133">**Pre Authentication**: How Application Proxy verifies users before giving them access to your application.</span></span> 

     - <span data-ttu-id="7619c-134">Azure Active Directory: Proxy aplikace přesměruje uživatele na stránku pro přihlášení ke službě Azure AD, která ověří jejich oprávnění k adresáři a aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7619c-134">Azure Active Directory: Application Proxy redirects users to sign in with Azure AD, which authenticates their permissions for the directory and application.</span></span> <span data-ttu-id="7619c-135">Doporučujeme zachovat tuto možnost jako výchozí, tak, aby můžete využít výhod funkcí zabezpečení Azure AD jako podmíněného přístupu a vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="7619c-135">We recommend keeping this option as the default, so that you can take advantage of Azure AD security features like conditional access and Multi-Factor Authentication.</span></span>
     - <span data-ttu-id="7619c-136">Průchod: Uživatelé nemají k ověřování na základě Azure Active Directory pro přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7619c-136">Passthrough: Users don't have to authenticate against Azure Active Directory to access the application.</span></span> <span data-ttu-id="7619c-137">Přesto můžete nastavit požadavky na ověřování na back-end.</span><span class="sxs-lookup"><span data-stu-id="7619c-137">You can still set up authentication requirements on the backend.</span></span>
   - <span data-ttu-id="7619c-138">**Konektor skupiny**: konektory zpracovat vzdálený přístup k vaší aplikaci a konektor skupiny k uspořádání konektory a aplikace podle oblasti, sítě nebo účel.</span><span class="sxs-lookup"><span data-stu-id="7619c-138">**Connector Group**: Connectors process the remote access to your application, and connector groups help you organize connectors and apps by region, network, or purpose.</span></span> <span data-ttu-id="7619c-139">Pokud nemáte žádné skupiny konektor dosud vytvořena, vaše aplikace je přiřazen k **výchozí**.</span><span class="sxs-lookup"><span data-stu-id="7619c-139">If you don't have any connector groups created yet, your app is assigned to **Default**.</span></span>

   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/configure-app.png)
5. <span data-ttu-id="7619c-141">V případě potřeby nakonfigurujte další nastavení.</span><span class="sxs-lookup"><span data-stu-id="7619c-141">If necessary, configure additional settings.</span></span> <span data-ttu-id="7619c-142">Pro většinu aplikací byste měli mít tato nastavení na jejich výchozí stavy.</span><span class="sxs-lookup"><span data-stu-id="7619c-142">For most applications, you should keep these settings in their default states.</span></span> 
   - <span data-ttu-id="7619c-143">**Časový limit pro aplikace k back-end**:, nastavte hodnotu **dlouho** pouze v případě, že aplikace je pomalá k ověření a připojení.</span><span class="sxs-lookup"><span data-stu-id="7619c-143">**Backend Application Timeout**: Set this value to **Long** only if your application is slow to authenticate and connect.</span></span> 
   - <span data-ttu-id="7619c-144">**Překlad adresy URL v záhlaví**: ponechte tuto hodnotu jako **Ano** Pokud aplikace nevyžaduje původní hlavičku hostitele v žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="7619c-144">**Translate URLs in Headers**: Keep this value as **Yes** unless your application required the original host header in the authentication request.</span></span>
   - <span data-ttu-id="7619c-145">**Překlad adres URL do těla aplikace**: ponechte tuto hodnotu jako **ne** Pokud máte pevně zakódované HTML odkazy na další místní aplikace a nepoužívejte vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="7619c-145">**Translate URLs in Application Body**: Keep this value as **No** unless you have hardcoded HTML links to other on-premises applications, and don't use custom domains.</span></span> <span data-ttu-id="7619c-146">Další informace najdete v tématu [odkaz překlad pomocí Proxy aplikace](application-proxy-link-translation.md).</span><span class="sxs-lookup"><span data-stu-id="7619c-146">For more information, see [Link translation with Application Proxy](application-proxy-link-translation.md).</span></span>
   
   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. <span data-ttu-id="7619c-148">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="7619c-148">Select **Add**.</span></span>


## <a name="add-a-test-user"></a><span data-ttu-id="7619c-149">Přidání testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7619c-149">Add a test user</span></span> 

<span data-ttu-id="7619c-150">Pokud chcete otestovat, že aplikace byla publikovaný správně, přidejte testovací uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="7619c-150">To test that your app was published correctly, add a test user account.</span></span> <span data-ttu-id="7619c-151">Ověřte, že tento účet již má oprávnění pro přístup k aplikaci uvnitř podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="7619c-151">Verify that this account already has permissions to access the app from inside the corporate network.</span></span>

1. <span data-ttu-id="7619c-152">Zpět v okně rychlý start vyberte **přiřadit uživatele pro testování**.</span><span class="sxs-lookup"><span data-stu-id="7619c-152">Back on the Quick start blade, select **Assign a user for testing**.</span></span>

  ![Přiřazení uživatele pro testování](./media/application-proxy-publish-azure-portal/assign-user.png)

2. <span data-ttu-id="7619c-154">Na uživatele a skupiny okně, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7619c-154">On the Users and groups blade, select **Add**.</span></span>

  ![Přidat uživatele nebo skupiny](./media/application-proxy-publish-azure-portal/add-user.png)

3. <span data-ttu-id="7619c-156">V okně Přidat přiřazení vyberte **uživatelů a skupin** potom vyberte účet, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="7619c-156">On the Add assignment blade, select **Users and groups** then choose the account you want to add.</span></span> 
4. <span data-ttu-id="7619c-157">Vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="7619c-157">Select **Assign**.</span></span>

## <a name="test-your-published-app"></a><span data-ttu-id="7619c-158">Testování publikované aplikace</span><span class="sxs-lookup"><span data-stu-id="7619c-158">Test your published app</span></span>

<span data-ttu-id="7619c-159">V prohlížeči přejděte na externí adresu URL, kterou jste nakonfigurovali během kroku publikovat.</span><span class="sxs-lookup"><span data-stu-id="7619c-159">In your browser, navigate to the external URL that you configured during the publish step.</span></span> <span data-ttu-id="7619c-160">By měl vidět na úvodní obrazovce a mít možnost se přihlásit pomocí účtu test, který nastavíte.</span><span class="sxs-lookup"><span data-stu-id="7619c-160">You should see the start screen, and be able to sign in with the test account you set up.</span></span>

![Testování publikované aplikace](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a><span data-ttu-id="7619c-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7619c-162">Next steps</span></span>
- <span data-ttu-id="7619c-163">[Stáhnout konektory](active-directory-application-proxy-enable.md) a [vytvořit konektor skupiny](active-directory-application-proxy-connectors-azure-portal.md) k publikování aplikací na samostatných sítí a umístění.</span><span class="sxs-lookup"><span data-stu-id="7619c-163">[Download connectors](active-directory-application-proxy-enable.md) and [create connector groups](active-directory-application-proxy-connectors-azure-portal.md) to publish applications on separate networks and locations.</span></span>

- <span data-ttu-id="7619c-164">[Nastavení jednotného přihlašování](application-proxy-sso-azure-portal.md) nově publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7619c-164">[Set up single sign-on](application-proxy-sso-azure-portal.md) for your newly published app</span></span>
