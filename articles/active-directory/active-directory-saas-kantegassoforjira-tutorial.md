---
title: "Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro JIRA | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kantega jednotné přihlašování pro JIRA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2af4891-e3c8-43b3-bdcb-0500c493e9b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 06a1d301818f025270137f7eaa9f40e5e4503112
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a><span data-ttu-id="e0b73-103">Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro JIRA</span><span class="sxs-lookup"><span data-stu-id="e0b73-103">Tutorial: Azure Active Directory integration with Kantega SSO for JIRA</span></span>

<span data-ttu-id="e0b73-104">V tomto kurzu zjistěte, jak integrovat Kantega jednotné přihlašování pro JIRA s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e0b73-104">In this tutorial, you learn how to integrate Kantega SSO for JIRA with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e0b73-105">Integrace Kantega jednotné přihlašování pro JIRA s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e0b73-105">Integrating Kantega SSO for JIRA with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e0b73-106">Můžete řídit ve službě Azure AD, kdo má přístup k Kantega jednotné přihlašování pro JIRA</span><span class="sxs-lookup"><span data-stu-id="e0b73-106">You can control in Azure AD who has access to Kantega SSO for JIRA</span></span>
- <span data-ttu-id="e0b73-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Kantega jednotné přihlašování pro JIRA (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0b73-107">You can enable your users to automatically get signed-on to Kantega SSO for JIRA (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e0b73-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e0b73-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e0b73-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e0b73-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0b73-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0b73-110">Prerequisites</span></span>

<span data-ttu-id="e0b73-111">Ke konfiguraci integrace služby Azure AD pomocí jednotného přihlašování Kantega pro JIRA, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-111">To configure Azure AD integration with Kantega SSO for JIRA, you need the following items:</span></span>

- <span data-ttu-id="e0b73-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0b73-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e0b73-113">Předplatné povolené SSO Kantega pro JIRA jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e0b73-113">A Kantega SSO for JIRA single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e0b73-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0b73-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e0b73-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e0b73-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e0b73-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e0b73-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e0b73-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0b73-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e0b73-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e0b73-118">Scenario description</span></span>
<span data-ttu-id="e0b73-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0b73-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e0b73-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e0b73-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e0b73-121">Přidání Kantega jednotné přihlašování pro JIRA z Galerie</span><span class="sxs-lookup"><span data-stu-id="e0b73-121">Adding Kantega SSO for JIRA from the gallery</span></span>
2. <span data-ttu-id="e0b73-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e0b73-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a><span data-ttu-id="e0b73-123">Přidání Kantega jednotné přihlašování pro JIRA z Galerie</span><span class="sxs-lookup"><span data-stu-id="e0b73-123">Adding Kantega SSO for JIRA from the gallery</span></span>
<span data-ttu-id="e0b73-124">Při konfiguraci integrace Kantega přihlašování pro JIRA do služby Azure AD, potřebujete přidat Kantega jednotné přihlašování pro JIRA z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e0b73-124">To configure the integration of Kantega SSO for JIRA into Azure AD, you need to add Kantega SSO for JIRA from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e0b73-125">**Pokud chcete přidat Kantega jednotné přihlašování pro JIRA z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e0b73-125">**To add Kantega SSO for JIRA from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e0b73-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e0b73-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e0b73-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e0b73-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e0b73-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e0b73-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e0b73-133">Do vyhledávacího pole zadejte **Kantega jednotné přihlašování pro JIRA**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-133">In the search box, type **Kantega SSO for JIRA**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_search.png)

5. <span data-ttu-id="e0b73-135">Na panelu výsledků vyberte **Kantega jednotné přihlašování pro JIRA**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e0b73-135">In the results panel, select **Kantega SSO for JIRA**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e0b73-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e0b73-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e0b73-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro JIRA podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e0b73-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for JIRA based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e0b73-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku SSO Kantega pro JIRA je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0b73-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kantega SSO for JIRA is to a user in Azure AD.</span></span> <span data-ttu-id="e0b73-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské SSO Kantega pro JIRA musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e0b73-140">In other words, a link relationship between an Azure AD user and the related user in Kantega SSO for JIRA needs to be established.</span></span>

<span data-ttu-id="e0b73-141">V Kantega jednotné přihlašování pro JIRA, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e0b73-141">In Kantega SSO for JIRA, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e0b73-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro JIRA, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-142">To configure and test Azure AD single sign-on with Kantega SSO for JIRA, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e0b73-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e0b73-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e0b73-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0b73-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e0b73-145">**[Vytváření Kantega SSO pro testovací uživatele JIRA](#creating-a-kantega-sso-for-jira-test-user)**  – Pokud chcete mít protějšek Britta Simon SSO Kantega pro JIRA propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e0b73-145">**[Creating a Kantega SSO for JIRA test user](#creating-a-kantega-sso-for-jira-test-user)** - to have a counterpart of Britta Simon in Kantega SSO for JIRA that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e0b73-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e0b73-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e0b73-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e0b73-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e0b73-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e0b73-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e0b73-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší Kantega jednotné přihlašování pro aplikace JIRA.</span><span class="sxs-lookup"><span data-stu-id="e0b73-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kantega SSO for JIRA application.</span></span>

<span data-ttu-id="e0b73-150">**Ke konfiguraci Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro JIRA, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e0b73-150">**To configure Azure AD single sign-on with Kantega SSO for JIRA, perform the following steps:**</span></span>

1. <span data-ttu-id="e0b73-151">Na portálu Azure na **Kantega jednotné přihlašování pro JIRA** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-151">In the Azure portal, on the **Kantega SSO for JIRA** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e0b73-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e0b73-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_samlbase.png)

3. <span data-ttu-id="e0b73-155">V **IDP** spustil v režimu **Kantega SSO JIRA domény a adresy URL** části provést následující krok:</span><span class="sxs-lookup"><span data-stu-id="e0b73-155">In **IDP** initiated mode, on the **Kantega SSO for JIRA Domain and URLs** section perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_url1.png)

    <span data-ttu-id="e0b73-157">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-157">a.</span></span> <span data-ttu-id="e0b73-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="e0b73-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="e0b73-159">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-159">b.</span></span> <span data-ttu-id="e0b73-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="e0b73-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="e0b73-161">V **SP** initiated režimu, zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provést následující krok:</span><span class="sxs-lookup"><span data-stu-id="e0b73-161">In **SP** initiated mode, check **Show advanced URL settings** and  perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_url2.png)

    <span data-ttu-id="e0b73-163">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="e0b73-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e0b73-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e0b73-164">These values are not real.</span></span> <span data-ttu-id="e0b73-165">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="e0b73-165">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="e0b73-166">Tyto hodnoty jsou přijímány během konfigurace modulu plug-in Jira, který je vysvětlen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e0b73-166">These values are received during the configuration of Jira plugin, which is explained later in the tutorial.</span></span>

5. <span data-ttu-id="e0b73-167">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e0b73-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_certificate.png) 

6. <span data-ttu-id="e0b73-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0b73-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="e0b73-171">V okně prohlížeče jiný web Přihlaste se k vaší JIRA na místním serveru jako správce.</span><span class="sxs-lookup"><span data-stu-id="e0b73-171">In a different web browser window, log in to your JIRA on premise server as an administrator.</span></span>

8. <span data-ttu-id="e0b73-172">Pozastavte ukazatel myši na ikonu a klikněte na **doplňky**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-172">Hover on cog and click the **Add-ons**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon1.png)

9. <span data-ttu-id="e0b73-174">Karta části doplňky, klikněte na tlačítko **najít nové rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-174">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="e0b73-175">Hledání **Kantega jednotné přihlašování pro JIRA (SAML & protokolu Kerberos)** a klikněte na tlačítko **nainstalovat** tlačítko k instalaci nové zásuvný modul SAML.</span><span class="sxs-lookup"><span data-stu-id="e0b73-175">Search **Kantega SSO for JIRA (SAML & Kerberos)** and click **Install** button to install the new SAML plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon2.png)

10. <span data-ttu-id="e0b73-177">Spustí se instalace modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="e0b73-177">The plugin installation starts.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon3.png)

11. <span data-ttu-id="e0b73-179">Po dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="e0b73-179">Once the installation is complete.</span></span> <span data-ttu-id="e0b73-180">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-180">Click **Close**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon33.png)

12. <span data-ttu-id="e0b73-182">Klikněte na **Manage** (Spravovat).</span><span class="sxs-lookup"><span data-stu-id="e0b73-182">Click **Manage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon34.png)
    
13. <span data-ttu-id="e0b73-184">Nový modul plug-in je uveden v části **integrace**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-184">New plugin is listed under **INTEGRATIONS**.</span></span> <span data-ttu-id="e0b73-185">Klikněte na tlačítko **konfigurace** konfigurace nového modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="e0b73-185">Click **Configure** to configure the new plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon35.png)

14. <span data-ttu-id="e0b73-187">V **SAML** části.</span><span class="sxs-lookup"><span data-stu-id="e0b73-187">In the **SAML** section.</span></span> <span data-ttu-id="e0b73-188">Vyberte **Azure Active Directory (Azure AD)** z **zprostředkovatele identity přidat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="e0b73-188">Select **Azure Active Directory (Azure AD)** from the **Add identity provider** dropdown.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon4.png)

15. <span data-ttu-id="e0b73-190">Vyberte úroveň předplatné jako **základní**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-190">Select subscription level as **Basic**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon5.png)     

16. <span data-ttu-id="e0b73-192">Na **vlastností aplikace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-192">On the **App properties** section, perform following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon6.png)

    <span data-ttu-id="e0b73-194">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-194">a.</span></span> <span data-ttu-id="e0b73-195">Kopírování **identifikátor ID URI aplikace** hodnotu a použít ho jako **identifikátor, adresa URL odpovědi a přihlašovací adresa URL** na **Kantega SSO JIRA domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e0b73-195">Copy the **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on the **Kantega SSO for JIRA Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="e0b73-196">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-196">b.</span></span> <span data-ttu-id="e0b73-197">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-197">Click **Next**.</span></span>

17. <span data-ttu-id="e0b73-198">Na **import metadat** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-198">On the **Metadata import** section, perform following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon7.png)

    <span data-ttu-id="e0b73-200">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-200">a.</span></span> <span data-ttu-id="e0b73-201">Vyberte **soubor metadat v mém počítači**a metadata souboru k odeslání, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e0b73-201">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="e0b73-202">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-202">b.</span></span> <span data-ttu-id="e0b73-203">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-203">Click **Next**.</span></span>

18. <span data-ttu-id="e0b73-204">Na **název a jednotného přihlašování k umístění** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-204">On the **Name and SSO location** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon8.png)
    
    <span data-ttu-id="e0b73-206">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-206">a.</span></span> <span data-ttu-id="e0b73-207">Přidejte název poskytovatele Identity **název zprostředkovatele Identity** textbox (např. Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e0b73-207">Add Name of the Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="e0b73-208">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-208">b.</span></span> <span data-ttu-id="e0b73-209">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-209">Click **Next**.</span></span>

19. <span data-ttu-id="e0b73-210">Ověřte podpisového certifikátu a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-210">Verify the Signing certificate and click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon9.png)

20. <span data-ttu-id="e0b73-212">Na **JIRA uživatelské účty** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-212">On the **JIRA user accounts** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon10.png)

    <span data-ttu-id="e0b73-214">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-214">a.</span></span> <span data-ttu-id="e0b73-215">Vyberte **v případě potřeby vytvořte uživatele v adresáři JIRA na interní** a zadejte odpovídající název skupiny pro uživatele (může být více ne.</span><span class="sxs-lookup"><span data-stu-id="e0b73-215">Select **Create users in JIRA's internal Directory if needed** and enter the appropriate name of the group for users (can be multiple no.</span></span> <span data-ttu-id="e0b73-216">skupin oddělené čárkou).</span><span class="sxs-lookup"><span data-stu-id="e0b73-216">of groups separated by comma).</span></span>

    <span data-ttu-id="e0b73-217">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-217">b.</span></span> <span data-ttu-id="e0b73-218">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-218">Click **Next**.</span></span>

21. <span data-ttu-id="e0b73-219">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-219">Click **Finish**.</span></span>   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon11.png)

22. <span data-ttu-id="e0b73-221">Na **známé domén pro Azure AD** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-221">On the **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/addon12.png)

    <span data-ttu-id="e0b73-223">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-223">a.</span></span> <span data-ttu-id="e0b73-224">Vyberte **známé domén** v levém panelu stránky.</span><span class="sxs-lookup"><span data-stu-id="e0b73-224">Select **Known domains** from the left panel of the page.</span></span>

    <span data-ttu-id="e0b73-225">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-225">b.</span></span> <span data-ttu-id="e0b73-226">Zadejte název domény v **známé domén** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e0b73-226">Enter domain name in the **Known domains** textbox.</span></span>

    <span data-ttu-id="e0b73-227">c.</span><span class="sxs-lookup"><span data-stu-id="e0b73-227">c.</span></span> <span data-ttu-id="e0b73-228">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-228">Click **Save**.</span></span> 

> [!TIP]
> <span data-ttu-id="e0b73-229">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e0b73-229">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e0b73-230">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e0b73-230">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e0b73-231">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e0b73-231">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e0b73-232">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0b73-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="e0b73-233">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0b73-233">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e0b73-235">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e0b73-235">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e0b73-236">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e0b73-236">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e0b73-238">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-238">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e0b73-240">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e0b73-240">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e0b73-242">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-242">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforjira-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e0b73-244">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-244">a.</span></span> <span data-ttu-id="e0b73-245">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-245">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e0b73-246">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-246">b.</span></span> <span data-ttu-id="e0b73-247">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e0b73-247">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e0b73-248">c.</span><span class="sxs-lookup"><span data-stu-id="e0b73-248">c.</span></span> <span data-ttu-id="e0b73-249">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-249">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e0b73-250">d.</span><span class="sxs-lookup"><span data-stu-id="e0b73-250">d.</span></span> <span data-ttu-id="e0b73-251">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-251">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-jira-test-user"></a><span data-ttu-id="e0b73-252">Vytváření SSO Kantega pro JIRA testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e0b73-252">Creating a Kantega SSO for JIRA test user</span></span>

<span data-ttu-id="e0b73-253">Pokud chcete povolit uživatelům Azure AD přihlášení k JIRA, musí být zřízená do JIRA.</span><span class="sxs-lookup"><span data-stu-id="e0b73-253">To enable Azure AD users to log in to JIRA, they must be provisioned into JIRA.</span></span> <span data-ttu-id="e0b73-254">V Kantega jednotné přihlašování pro JIRA zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="e0b73-254">In Kantega SSO for JIRA, provisioning is a manual task.</span></span>

<span data-ttu-id="e0b73-255">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e0b73-255">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="e0b73-256">Přihlaste se k vaší JIRA na místním serveru jako správce.</span><span class="sxs-lookup"><span data-stu-id="e0b73-256">Log in to your JIRA on premise server as an administrator.</span></span>

2. <span data-ttu-id="e0b73-257">Pozastavte ukazatel myši na ikonu a klikněte na **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-257">Hover on cog and click the **User management**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforjira-tutorial/user1.png) 

3. <span data-ttu-id="e0b73-259">V části **Správa uživatelů** oddíl, klikněte na **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-259">Under **User management** tab section, click **Create user**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforjira-tutorial/user2.png) 

4. <span data-ttu-id="e0b73-261">Na **"Vytvořit nový uživatel"** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e0b73-261">On the **“Create new user”** dialog page, perform the following steps:</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforjira-tutorial/user3.png) 

    <span data-ttu-id="e0b73-263">a.</span><span class="sxs-lookup"><span data-stu-id="e0b73-263">a.</span></span> <span data-ttu-id="e0b73-264">V **e-mailová adresa** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="e0b73-264">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="e0b73-265">b.</span><span class="sxs-lookup"><span data-stu-id="e0b73-265">b.</span></span> <span data-ttu-id="e0b73-266">V **úplný název** textovému poli, úplný název typu uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0b73-266">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>

    <span data-ttu-id="e0b73-267">c.</span><span class="sxs-lookup"><span data-stu-id="e0b73-267">c.</span></span> <span data-ttu-id="e0b73-268">V **uživatelské jméno** jako typ e-mailu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="e0b73-268">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="e0b73-269">d.</span><span class="sxs-lookup"><span data-stu-id="e0b73-269">d.</span></span> <span data-ttu-id="e0b73-270">V **heslo** textovému poli, zadejte heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="e0b73-270">In the **Password** textbox, type the password of user.</span></span>

    <span data-ttu-id="e0b73-271">e.</span><span class="sxs-lookup"><span data-stu-id="e0b73-271">e.</span></span> <span data-ttu-id="e0b73-272">Klikněte na tlačítko **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-272">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e0b73-273">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0b73-273">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e0b73-274">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí jednotného přihlašování k Kantega pro JIRA udělení přístupu.</span><span class="sxs-lookup"><span data-stu-id="e0b73-274">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kantega SSO for JIRA.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e0b73-276">**Pokud chcete přiřadit Britta Simon Kantega jednotné přihlašování pro JIRA, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e0b73-276">**To assign Britta Simon to Kantega SSO for JIRA, perform the following steps:**</span></span>

1. <span data-ttu-id="e0b73-277">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-277">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e0b73-279">V seznamu aplikací vyberte **Kantega jednotné přihlašování pro JIRA**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-279">In the applications list, select **Kantega SSO for JIRA**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_kantegassoforjira_app.png) 

3. <span data-ttu-id="e0b73-281">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e0b73-281">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e0b73-283">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0b73-283">Click **Add** button.</span></span> <span data-ttu-id="e0b73-284">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e0b73-284">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e0b73-286">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e0b73-286">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e0b73-287">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e0b73-287">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e0b73-288">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e0b73-288">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e0b73-289">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e0b73-289">Testing single sign-on</span></span>

<span data-ttu-id="e0b73-290">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e0b73-290">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e0b73-291">Po kliknutí na tlačítko Kantega jednotné přihlašování pro dlaždici JIRA na přístupovém panelu, můžete by měl získat automaticky přihlášení k vaší Kantega jednotné přihlašování pro aplikace JIRA.</span><span class="sxs-lookup"><span data-stu-id="e0b73-291">When you click the Kantega SSO for JIRA tile in the Access Panel, you should get automatically signed-on to your Kantega SSO for JIRA application.</span></span>
<span data-ttu-id="e0b73-292">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e0b73-292">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e0b73-293">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e0b73-293">Additional resources</span></span>

* [<span data-ttu-id="e0b73-294">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0b73-294">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e0b73-295">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e0b73-295">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforjira-tutorial/tutorial_general_203.png

