---
title: "aaaPublish aplikací pomocí proxy aplikace služby Azure AD | Microsoft Docs"
description: "Publikujte místní aplikace toohello cloudu s Azure AD Application Proxy v hello portálu Azure."
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
ms.openlocfilehash: ed5458467fb7d4376f65a222f1ba5f23cfdfc57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="45b5e-103">Publikování aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="45b5e-103">Publish applications using Azure AD Application Proxy</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="45b5e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="45b5e-104">Azure portal</span></span>](application-proxy-publish-azure-portal.md)
> * [<span data-ttu-id="45b5e-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="45b5e-105">Azure classic portal</span></span>](active-directory-application-proxy-publish.md)

<span data-ttu-id="45b5e-106">Proxy aplikace služby Azure Active Directory (AD) umožňuje podporu zaměstnanci na vzdálených pracovištích tím, že publikujete místní aplikace toobe přístupné přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="45b5e-106">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications toobe accessed over hello internet.</span></span> <span data-ttu-id="45b5e-107">Můžete publikovat tyto aplikace prostřednictvím hello Azure portálu tooprovide zabezpečený vzdálený přístup mimo vaši síť.</span><span class="sxs-lookup"><span data-stu-id="45b5e-107">You can publish these applications through hello Azure portal tooprovide secure remote access from outside your network.</span></span>

<span data-ttu-id="45b5e-108">Tento článek vás provede kroky toopublish hello místní aplikace pomocí Proxy aplikací.</span><span class="sxs-lookup"><span data-stu-id="45b5e-108">This article walks you through hello steps toopublish an on-premises app with Application Proxy.</span></span> <span data-ttu-id="45b5e-109">Po dokončení tohoto článku, uživatelé budou mít možnost tooaccess aplikace vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="45b5e-109">After you complete this article, your users will be able tooaccess your app remotely.</span></span> <span data-ttu-id="45b5e-110">A budete mít připravené tooconfigure další funkce pro aplikaci hello jako jednotné přihlašování, přizpůsobené informace a požadavky na zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="45b5e-110">And you'll be ready tooconfigure extra features for hello application like single sign-on, personalized information, and security requirements.</span></span>

<span data-ttu-id="45b5e-111">Pokud jste nový tooApplication Proxy, další informace o této funkci s hello článku [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="45b5e-111">If you're new tooApplication Proxy, learn more about this feature with hello article [How tooprovide secure remote access tooon-premises applications](active-directory-application-proxy-get-started.md).</span></span>


## <a name="publish-an-on-premises-app-for-remote-access"></a><span data-ttu-id="45b5e-112">Publikování aplikace místně pro vzdálený přístup</span><span class="sxs-lookup"><span data-stu-id="45b5e-112">Publish an on-premises app for remote access</span></span>

<span data-ttu-id="45b5e-113">Postupujte podle těchto kroků toopublish aplikací pomocí Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="45b5e-113">Follow these steps toopublish your apps with Application Proxy.</span></span> <span data-ttu-id="45b5e-114">Pokud už nejsou stáhnout a nakonfigurovat konektor pro vaši organizaci, přejděte příliš[začít pracovat s Proxy aplikace a nainstalujte konektor hello](active-directory-application-proxy-enable.md) první a pak publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="45b5e-114">If you haven't already downloaded and configured a connector for your organization, go too[Get started with Application Proxy and install hello connector](active-directory-application-proxy-enable.md) first, and then publish your app.</span></span>

> [!TIP]
> <span data-ttu-id="45b5e-115">Pokud testujete aplikace Proxy pro hello poprvé, vyberte aplikaci, která je nastavena pro ověřování pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="45b5e-115">If you're testing out Application Proxy for hello first time, choose an application that's set up for password-based authentication.</span></span> <span data-ttu-id="45b5e-116">Proxy aplikací podporuje další typy ověřování, ale jsou založené na heslech aplikace hello nejjednodušší tooget si rychle a spustit.</span><span class="sxs-lookup"><span data-stu-id="45b5e-116">Application Proxy supports other types of authentication, but password-based apps are hello easiest tooget up and running quickly.</span></span> 

1. <span data-ttu-id="45b5e-117">Přihlaste se jako správce v hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="45b5e-117">Sign in as an administrator in hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="45b5e-118">Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="45b5e-118">Select **Azure Active Directory** > **Enterprise applications** > **New application**.</span></span>

  ![Přidat podniková aplikace](./media/application-proxy-publish-azure-portal/add-app.png)

3. <span data-ttu-id="45b5e-120">Vyberte **všechny**, pak vyberte **místní aplikace**.</span><span class="sxs-lookup"><span data-stu-id="45b5e-120">Select **All**, then select **On-premises application**.</span></span>  

  ![Přidat vlastní aplikaci](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. <span data-ttu-id="45b5e-122">Zadejte následující informace o vaší aplikaci hello:</span><span class="sxs-lookup"><span data-stu-id="45b5e-122">Provide hello following information about your application:</span></span>

   - <span data-ttu-id="45b5e-123">**Název**: název hello hello aplikace, která se zobrazí na panelu hello přístupu a v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="45b5e-123">**Name**: hello name of hello application that will appear on hello access panel and in hello Azure portal.</span></span> 

   - <span data-ttu-id="45b5e-124">**Interní adresa URL**: hello URL, kterou používají aplikace hello tooaccess uvnitř vaší privátní sítě.</span><span class="sxs-lookup"><span data-stu-id="45b5e-124">**Internal URL**: hello URL that you use tooaccess hello application from inside your private network.</span></span> <span data-ttu-id="45b5e-125">Při publikování hello zbytek hello serveru, můžete zadat konkrétní cestu na hello back-end serveru toopublish.</span><span class="sxs-lookup"><span data-stu-id="45b5e-125">You can provide a specific path on hello backend server toopublish, while hello rest of hello server is unpublished.</span></span> <span data-ttu-id="45b5e-126">Tímto způsobem můžete publikovat různé stránky na stejném serveru jako různé aplikace hello a poskytněte každé z nich vlastní název a pravidla přístupu.</span><span class="sxs-lookup"><span data-stu-id="45b5e-126">In this way, you can publish different sites on hello same server as different apps, and give each one its own name and access rules.</span></span>

     > [!TIP]
     > <span data-ttu-id="45b5e-127">Pokud publikujete cestu, ujistěte se, že obsahuje všechny potřebné obrázků hello, skriptů a stylů pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="45b5e-127">If you publish a path, make sure that it includes all hello necessary images, scripts, and style sheets for your application.</span></span> <span data-ttu-id="45b5e-128">Například pokud vaše aplikace je v https://yourapp/app a používá nacházející se v https://yourapp/media bitové kopie, pak byste měli publikovat https://yourapp/ jako cestu hello.</span><span class="sxs-lookup"><span data-stu-id="45b5e-128">For example, if your app is at https://yourapp/app and uses images located at https://yourapp/media, then you should publish https://yourapp/ as hello path.</span></span> <span data-ttu-id="45b5e-129">Tato interní adresa URL nemá toobe hello cílová stránka, která se uživatelům zobrazí.</span><span class="sxs-lookup"><span data-stu-id="45b5e-129">This internal URL doesn't have toobe hello landing page your users see.</span></span> <span data-ttu-id="45b5e-130">Další informace najdete v tématu [nastavit vlastní domovskou stránku pro publikované aplikace](application-proxy-office365-app-launcher.md).</span><span class="sxs-lookup"><span data-stu-id="45b5e-130">For more information, see [Set a custom home page for published apps](application-proxy-office365-app-launcher.md).</span></span>

   - <span data-ttu-id="45b5e-131">**Externí adresu URL**: hello adresu vaši uživatelé pak budou muset tooin pořadí tooaccess hello aplikace z mimo vaši síť.</span><span class="sxs-lookup"><span data-stu-id="45b5e-131">**External URL**: hello address your users will go tooin order tooaccess hello app from outside your network.</span></span> <span data-ttu-id="45b5e-132">Pokud nechcete, aby toouse hello výchozí Proxy aplikace doménu, přečtěte si informace o [vlastních domén v Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).</span><span class="sxs-lookup"><span data-stu-id="45b5e-132">If you don't want toouse hello default Application Proxy domain, read about [custom domains in Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).</span></span>
   - <span data-ttu-id="45b5e-133">**Předběžné ověřování**: jak Proxy aplikace ověřuje uživatele před poskytnete jim přístup tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="45b5e-133">**Pre Authentication**: How Application Proxy verifies users before giving them access tooyour application.</span></span> 

     - <span data-ttu-id="45b5e-134">Azure Active Directory: Proxy aplikace přesměruje uživatele toosign pomocí Azure AD, která ověří jejich oprávnění k hello adresáři a aplikaci.</span><span class="sxs-lookup"><span data-stu-id="45b5e-134">Azure Active Directory: Application Proxy redirects users toosign in with Azure AD, which authenticates their permissions for hello directory and application.</span></span> <span data-ttu-id="45b5e-135">Doporučujeme zachovat tuto možnost jako výchozí hello tak, aby můžete využít výhod funkcí zabezpečení Azure AD jako podmíněného přístupu a vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="45b5e-135">We recommend keeping this option as hello default, so that you can take advantage of Azure AD security features like conditional access and Multi-Factor Authentication.</span></span>
     - <span data-ttu-id="45b5e-136">Průchod: Uživatelé nemají tooauthenticate proti aplikace hello tooaccess Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="45b5e-136">Passthrough: Users don't have tooauthenticate against Azure Active Directory tooaccess hello application.</span></span> <span data-ttu-id="45b5e-137">Přesto můžete nastavit požadavky na ověřování na back-end hello.</span><span class="sxs-lookup"><span data-stu-id="45b5e-137">You can still set up authentication requirements on hello backend.</span></span>
   - <span data-ttu-id="45b5e-138">**Konektor skupiny**: konektory proces hello vzdáleného přístupu tooyour aplikace a konektor skupiny vám pomáhají uspořádat konektory a aplikace podle oblasti, sítě nebo účel.</span><span class="sxs-lookup"><span data-stu-id="45b5e-138">**Connector Group**: Connectors process hello remote access tooyour application, and connector groups help you organize connectors and apps by region, network, or purpose.</span></span> <span data-ttu-id="45b5e-139">Pokud nemáte žádné skupiny konektor dosud vytvořena, vaše aplikace je příliš přiřazen**výchozí**.</span><span class="sxs-lookup"><span data-stu-id="45b5e-139">If you don't have any connector groups created yet, your app is assigned too**Default**.</span></span>

   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/configure-app.png)
5. <span data-ttu-id="45b5e-141">V případě potřeby nakonfigurujte další nastavení.</span><span class="sxs-lookup"><span data-stu-id="45b5e-141">If necessary, configure additional settings.</span></span> <span data-ttu-id="45b5e-142">Pro většinu aplikací byste měli mít tato nastavení na jejich výchozí stavy.</span><span class="sxs-lookup"><span data-stu-id="45b5e-142">For most applications, you should keep these settings in their default states.</span></span> 
   - <span data-ttu-id="45b5e-143">**Časový limit pro aplikace k back-end**: nastavení této hodnoty příliš**dlouho** pouze v případě, že aplikace je pomalá tooauthenticate a připojení.</span><span class="sxs-lookup"><span data-stu-id="45b5e-143">**Backend Application Timeout**: Set this value too**Long** only if your application is slow tooauthenticate and connect.</span></span> 
   - <span data-ttu-id="45b5e-144">**Překlad adresy URL v záhlaví**: ponechte tuto hodnotu jako **Ano** Pokud aplikace nevyžaduje hello původní hlavičku hostitele v žádosti o ověření hello.</span><span class="sxs-lookup"><span data-stu-id="45b5e-144">**Translate URLs in Headers**: Keep this value as **Yes** unless your application required hello original host header in hello authentication request.</span></span>
   - <span data-ttu-id="45b5e-145">**Překlad adres URL do těla aplikace**: ponechte tuto hodnotu jako **ne** Pokud máte pevně zakódované HTML odkazy tooother místní aplikace a nepoužívejte vlastní domény.</span><span class="sxs-lookup"><span data-stu-id="45b5e-145">**Translate URLs in Application Body**: Keep this value as **No** unless you have hardcoded HTML links tooother on-premises applications, and don't use custom domains.</span></span> <span data-ttu-id="45b5e-146">Další informace najdete v tématu [odkaz překlad pomocí Proxy aplikace](application-proxy-link-translation.md).</span><span class="sxs-lookup"><span data-stu-id="45b5e-146">For more information, see [Link translation with Application Proxy](application-proxy-link-translation.md).</span></span>
   
   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. <span data-ttu-id="45b5e-148">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="45b5e-148">Select **Add**.</span></span>


## <a name="add-a-test-user"></a><span data-ttu-id="45b5e-149">Přidání testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="45b5e-149">Add a test user</span></span> 

<span data-ttu-id="45b5e-150">tootest, že aplikace byla publikovaný správně, přidejte testovací uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="45b5e-150">tootest that your app was published correctly, add a test user account.</span></span> <span data-ttu-id="45b5e-151">Ověřte, že tento účet již má oprávnění tooaccess hello aplikace z uvnitř podnikové sítě hello.</span><span class="sxs-lookup"><span data-stu-id="45b5e-151">Verify that this account already has permissions tooaccess hello app from inside hello corporate network.</span></span>

1. <span data-ttu-id="45b5e-152">Zpět v okně rychlý start hello, vyberte **přiřadit uživatele pro testování**.</span><span class="sxs-lookup"><span data-stu-id="45b5e-152">Back on hello Quick start blade, select **Assign a user for testing**.</span></span>

  ![Přiřazení uživatele pro testování](./media/application-proxy-publish-azure-portal/assign-user.png)

2. <span data-ttu-id="45b5e-154">Hello uživatelů a skupin okně, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="45b5e-154">On hello Users and groups blade, select **Add**.</span></span>

  ![Přidat uživatele nebo skupiny](./media/application-proxy-publish-azure-portal/add-user.png)

3. <span data-ttu-id="45b5e-156">V okně přiřazení hello přidat, vyberte **uživatelů a skupin** zvolte účet hello chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="45b5e-156">On hello Add assignment blade, select **Users and groups** then choose hello account you want tooadd.</span></span> 
4. <span data-ttu-id="45b5e-157">Vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="45b5e-157">Select **Assign**.</span></span>

## <a name="test-your-published-app"></a><span data-ttu-id="45b5e-158">Testování publikované aplikace</span><span class="sxs-lookup"><span data-stu-id="45b5e-158">Test your published app</span></span>

<span data-ttu-id="45b5e-159">V prohlížeči přejděte externí adresu URL toohello, který jste nakonfigurovali během hello publikování krok.</span><span class="sxs-lookup"><span data-stu-id="45b5e-159">In your browser, navigate toohello external URL that you configured during hello publish step.</span></span> <span data-ttu-id="45b5e-160">Byste měli vidět hello úvodní obrazovce a být schopný toosign se pomocí účtu testovací hello nastavíte.</span><span class="sxs-lookup"><span data-stu-id="45b5e-160">You should see hello start screen, and be able toosign in with hello test account you set up.</span></span>

![Testování publikované aplikace](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a><span data-ttu-id="45b5e-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45b5e-162">Next steps</span></span>
- <span data-ttu-id="45b5e-163">[Stáhnout konektory](active-directory-application-proxy-enable.md) a [vytvořit konektor skupiny](active-directory-application-proxy-connectors-azure-portal.md) toopublish aplikací na samostatných sítí a umístění.</span><span class="sxs-lookup"><span data-stu-id="45b5e-163">[Download connectors](active-directory-application-proxy-enable.md) and [create connector groups](active-directory-application-proxy-connectors-azure-portal.md) toopublish applications on separate networks and locations.</span></span>

- <span data-ttu-id="45b5e-164">[Nastavení jednotného přihlašování](application-proxy-sso-azure-portal.md) nově publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="45b5e-164">[Set up single sign-on](application-proxy-sso-azure-portal.md) for your newly published app</span></span>
