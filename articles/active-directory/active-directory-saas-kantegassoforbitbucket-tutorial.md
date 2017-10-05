---
title: "Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro Bitbucket | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kantega jednotné přihlašování pro Bitbucket."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c41cdaaf-0441-493c-94c7-569615b7b1ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 6656c9abf8483ee98c0cb1a16c06d078e32240f2
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bitbucket"></a><span data-ttu-id="a995c-103">Kurz: Azure Active Directory integrace s Kantega jednotné přihlašování pro Bitbucket</span><span class="sxs-lookup"><span data-stu-id="a995c-103">Tutorial: Azure Active Directory integration with Kantega SSO for Bitbucket</span></span>

<span data-ttu-id="a995c-104">V tomto kurzu zjistěte, jak integrovat Kantega jednotné přihlašování pro Bitbucket s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a995c-104">In this tutorial, you learn how to integrate Kantega SSO for Bitbucket with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a995c-105">Integrace Kantega jednotné přihlašování pro Bitbucket s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a995c-105">Integrating Kantega SSO for Bitbucket with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a995c-106">Můžete řídit ve službě Azure AD, kdo má přístup k Kantega jednotné přihlašování pro Bitbucket</span><span class="sxs-lookup"><span data-stu-id="a995c-106">You can control in Azure AD who has access to Kantega SSO for Bitbucket</span></span>
- <span data-ttu-id="a995c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Kantega jednotné přihlašování pro Bitbucket (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a995c-107">You can enable your users to automatically get signed-on to Kantega SSO for Bitbucket (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a995c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a995c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a995c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a995c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a995c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a995c-110">Prerequisites</span></span>

<span data-ttu-id="a995c-111">Ke konfiguraci integrace služby Azure AD pomocí jednotného přihlašování Kantega pro Bitbucket, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a995c-111">To configure Azure AD integration with Kantega SSO for Bitbucket, you need the following items:</span></span>

- <span data-ttu-id="a995c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a995c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a995c-113">Předplatné povolené SSO Kantega pro Bitbucket jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a995c-113">A Kantega SSO for Bitbucket single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a995c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a995c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a995c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a995c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a995c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a995c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a995c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a995c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a995c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a995c-118">Scenario description</span></span>
<span data-ttu-id="a995c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a995c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a995c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a995c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a995c-121">Přidání Kantega jednotné přihlašování pro Bitbucket z Galerie</span><span class="sxs-lookup"><span data-stu-id="a995c-121">Adding Kantega SSO for Bitbucket from the gallery</span></span>
2. <span data-ttu-id="a995c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a995c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-bitbucket-from-the-gallery"></a><span data-ttu-id="a995c-123">Přidání Kantega jednotné přihlašování pro Bitbucket z Galerie</span><span class="sxs-lookup"><span data-stu-id="a995c-123">Adding Kantega SSO for Bitbucket from the gallery</span></span>
<span data-ttu-id="a995c-124">Při konfiguraci integrace Kantega přihlašování pro Bitbucket do služby Azure AD, potřebujete přidat Kantega jednotné přihlašování pro Bitbucket z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a995c-124">To configure the integration of Kantega SSO for Bitbucket into Azure AD, you need to add Kantega SSO for Bitbucket from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a995c-125">**Pokud chcete přidat Kantega jednotné přihlašování pro Bitbucket z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a995c-125">**To add Kantega SSO for Bitbucket from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a995c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a995c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a995c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a995c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a995c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a995c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a995c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a995c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a995c-133">Do vyhledávacího pole zadejte **Kantega jednotné přihlašování pro Bitbucket**.</span><span class="sxs-lookup"><span data-stu-id="a995c-133">In the search box, type **Kantega SSO for Bitbucket**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_search.png)

5. <span data-ttu-id="a995c-135">Na panelu výsledků vyberte **Kantega jednotné přihlašování pro Bitbucket**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a995c-135">In the results panel, select **Kantega SSO for Bitbucket**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a995c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a995c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a995c-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro Bitbucket podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a995c-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for Bitbucket based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a995c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku SSO Kantega pro Bitbucket je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a995c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kantega SSO for Bitbucket is to a user in Azure AD.</span></span> <span data-ttu-id="a995c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské SSO Kantega pro Bitbucket musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a995c-140">In other words, a link relationship between an Azure AD user and the related user in Kantega SSO for Bitbucket needs to be established.</span></span>

<span data-ttu-id="a995c-141">V Kantega jednotné přihlašování pro Bitbucket, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a995c-141">In Kantega SSO for Bitbucket, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a995c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro Bitbucket, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a995c-142">To configure and test Azure AD single sign-on with Kantega SSO for Bitbucket, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a995c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a995c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a995c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a995c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a995c-145">**[Vytváření Kantega SSO pro testovací uživatele Bitbucket](#creating-a-kantega-sso-for-bitbucket-test-user)**  – Pokud chcete mít protějšek Britta Simon SSO Kantega pro Bitbucket, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a995c-145">**[Creating a Kantega SSO for Bitbucket test user](#creating-a-kantega-sso-for-bitbucket-test-user)** - to have a counterpart of Britta Simon in Kantega SSO for Bitbucket that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a995c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a995c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a995c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a995c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a995c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a995c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a995c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší Kantega jednotné přihlašování pro aplikace Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="a995c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kantega SSO for Bitbucket application.</span></span>

<span data-ttu-id="a995c-150">**Ke konfiguraci Azure AD jednotné přihlašování pomocí jednotného přihlašování Kantega pro Bitbucket, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a995c-150">**To configure Azure AD single sign-on with Kantega SSO for Bitbucket, perform the following steps:**</span></span>

1. <span data-ttu-id="a995c-151">Na portálu Azure na **Kantega jednotné přihlašování pro Bitbucket** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a995c-151">In the Azure portal, on the **Kantega SSO for Bitbucket** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a995c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a995c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_samlbase.png)

3. <span data-ttu-id="a995c-155">V **IDP** spustil v režimu **Kantega SSO Bitbucket domény a adresy URL** části provést následující krok:</span><span class="sxs-lookup"><span data-stu-id="a995c-155">In **IDP** initiated mode, on the **Kantega SSO for Bitbucket Domain and URLs** section perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url1.png)

    <span data-ttu-id="a995c-157">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-157">a.</span></span> <span data-ttu-id="a995c-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="a995c-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="a995c-159">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-159">b.</span></span> <span data-ttu-id="a995c-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="a995c-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="a995c-161">V **SP** initiated režimu, zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provést následující krok:</span><span class="sxs-lookup"><span data-stu-id="a995c-161">In **SP** initiated mode, check **Show advanced URL settings** and  perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url2.png)
    
    <span data-ttu-id="a995c-163">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="a995c-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a995c-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a995c-164">These values are not real.</span></span> <span data-ttu-id="a995c-165">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="a995c-165">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="a995c-166">Tyto hodnoty jsou přijímány během konfigurace modulu plug-in Bitbucket, který je vysvětlen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a995c-166">These values are recieved during the configuration of Bitbucket plugin which is explained later in the tutorial.</span></span>

5. <span data-ttu-id="a995c-167">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a995c-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_certificate.png) 

6. <span data-ttu-id="a995c-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a995c-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a995c-171">V okně prohlížeče jiný web Přihlaste se k portálu správy Bitbucket jako správce.</span><span class="sxs-lookup"><span data-stu-id="a995c-171">In a different web browser window, log in to your Bitbucket admin portal as an administrator.</span></span>

8. <span data-ttu-id="a995c-172">Klikněte na ikonu a klikněte na **najít nové rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="a995c-172">Click cog and click the **Find new add-ons**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon1.png)

9. <span data-ttu-id="a995c-174">Hledání **Kantega jednotné přihlašování pro Bitbucket SAML & protokolu Kerberos** a klikněte na tlačítko **nainstalovat** tlačítko k instalaci nové zásuvný modul SAML.</span><span class="sxs-lookup"><span data-stu-id="a995c-174">Search **Kantega SSO for Bitbucket SAML & Kerberos** and click **Install** button to install the new SAML plugin.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon2.png)

10. <span data-ttu-id="a995c-176">Spustí se instalace modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="a995c-176">The plugin installation starts.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon31.png)

11. <span data-ttu-id="a995c-178">Po dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="a995c-178">Once the installation is complete.</span></span> <span data-ttu-id="a995c-179">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="a995c-179">Click **Close**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon33.png)

12. <span data-ttu-id="a995c-181">Klikněte na **Manage** (Spravovat).</span><span class="sxs-lookup"><span data-stu-id="a995c-181">Click **Manage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon34.png)
    
13. <span data-ttu-id="a995c-183">Klikněte na tlačítko **konfigurace** konfigurace nového modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="a995c-183">Click **Configure** to configure the new plugin.</span></span>    

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon35.png)

14. <span data-ttu-id="a995c-185">V **SAML** části.</span><span class="sxs-lookup"><span data-stu-id="a995c-185">In the **SAML** section.</span></span> <span data-ttu-id="a995c-186">Vyberte **Azure Active Directory (Azure AD)** z **zprostředkovatele identity přidat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="a995c-186">Select **Azure Active Directory (Azure AD)** from the **Add identity provider** dropdown.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon4.png)

15. <span data-ttu-id="a995c-188">Vyberte úroveň předplatné jako **základní**.</span><span class="sxs-lookup"><span data-stu-id="a995c-188">Select subscription level as **Basic**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon5.png)

16. <span data-ttu-id="a995c-190">Na **vlastností aplikace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a995c-190">On the **App properties** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon6.png)

    <span data-ttu-id="a995c-192">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-192">a.</span></span> <span data-ttu-id="a995c-193">Kopírování **identifikátor ID URI aplikace** hodnotu a použít ho jako **identifikátor, adresa URL odpovědi a přihlašovací adresa URL** na **Kantega SSO Bitbucket domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a995c-193">Copy the **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on the **Kantega SSO for Bitbucket Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="a995c-194">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-194">b.</span></span> <span data-ttu-id="a995c-195">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a995c-195">Click **Next**.</span></span>

17. <span data-ttu-id="a995c-196">Na **import metadat** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a995c-196">On the **Metadata import** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon7.png)

    <span data-ttu-id="a995c-198">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-198">a.</span></span> <span data-ttu-id="a995c-199">Vyberte **soubor metadat v mém počítači**a metadata souboru k odeslání, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a995c-199">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="a995c-200">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-200">b.</span></span> <span data-ttu-id="a995c-201">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a995c-201">Click **Next**.</span></span>

18. <span data-ttu-id="a995c-202">Na **název a jednotného přihlašování k umístění** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a995c-202">On the **Name and SSO location** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon8.png)

    <span data-ttu-id="a995c-204">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-204">a.</span></span> <span data-ttu-id="a995c-205">Přidejte název poskytovatele Identity **název zprostředkovatele Identity** textbox (např. Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a995c-205">Add Name of the Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="a995c-206">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-206">b.</span></span> <span data-ttu-id="a995c-207">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a995c-207">Click **Next**.</span></span>

19. <span data-ttu-id="a995c-208">Ověřte podpisového certifikátu a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a995c-208">Verify the Signing certificate and click **Next**.</span></span>  

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon9.png)

20. <span data-ttu-id="a995c-210">Na **Bitbucket uživatelské účty** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a995c-210">On the **Bitbucket user accounts** section, perform following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon10.png)

    <span data-ttu-id="a995c-212">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-212">a.</span></span> <span data-ttu-id="a995c-213">Vyberte **v případě potřeby vytvořte uživatele v adresáři Bitbucket na interní** a zadejte odpovídající název skupiny pro uživatele (může být více ne.</span><span class="sxs-lookup"><span data-stu-id="a995c-213">Select **Create users in Bitbucket's internal Directory if needed** and enter the appropriate name of the group for users (can be multiple no.</span></span> <span data-ttu-id="a995c-214">skupin oddělené čárkou).</span><span class="sxs-lookup"><span data-stu-id="a995c-214">of groups separated by comma).</span></span>

    <span data-ttu-id="a995c-215">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-215">b.</span></span> <span data-ttu-id="a995c-216">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a995c-216">Click **Next**.</span></span>

21. <span data-ttu-id="a995c-217">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a995c-217">Click **Finish**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon11.png)

22. <span data-ttu-id="a995c-219">Na **známé domén pro Azure AD** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a995c-219">On the **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon12.png)

    <span data-ttu-id="a995c-221">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-221">a.</span></span> <span data-ttu-id="a995c-222">Vyberte **známé domén** v levém panelu stránky.</span><span class="sxs-lookup"><span data-stu-id="a995c-222">Select **Known domains** from the left panel of the page.</span></span>

    <span data-ttu-id="a995c-223">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-223">b.</span></span> <span data-ttu-id="a995c-224">Zadejte název domény v **známé domén** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a995c-224">Enter domain name in the **Known domains** textbox.</span></span>

    <span data-ttu-id="a995c-225">c.</span><span class="sxs-lookup"><span data-stu-id="a995c-225">c.</span></span> <span data-ttu-id="a995c-226">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a995c-226">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="a995c-227">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a995c-227">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a995c-228">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a995c-228">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a995c-229">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a995c-229">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a995c-230">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a995c-230">Creating an Azure AD test user</span></span>
<span data-ttu-id="a995c-231">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a995c-231">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a995c-233">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a995c-233">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a995c-234">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a995c-234">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a995c-236">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a995c-236">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a995c-238">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a995c-238">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a995c-240">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a995c-240">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a995c-242">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-242">a.</span></span> <span data-ttu-id="a995c-243">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a995c-243">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a995c-244">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-244">b.</span></span> <span data-ttu-id="a995c-245">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a995c-245">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a995c-246">c.</span><span class="sxs-lookup"><span data-stu-id="a995c-246">c.</span></span> <span data-ttu-id="a995c-247">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a995c-247">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a995c-248">d.</span><span class="sxs-lookup"><span data-stu-id="a995c-248">d.</span></span> <span data-ttu-id="a995c-249">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a995c-249">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-bitbucket-test-user"></a><span data-ttu-id="a995c-250">Vytváření SSO Kantega pro Bitbucket testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a995c-250">Creating a Kantega SSO for Bitbucket test user</span></span>

<span data-ttu-id="a995c-251">Pokud chcete povolit uživatelům Azure AD přihlášení k Bitbucket, musí být zřízená do Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="a995c-251">To enable Azure AD users to log in to Bitbucket, they must be provisioned into Bitbucket.</span></span> <span data-ttu-id="a995c-252">V Kantega jednotné přihlašování pro Bitbucket zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a995c-252">In Kantega SSO for Bitbucket, provisioning is a manual task.</span></span>

<span data-ttu-id="a995c-253">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a995c-253">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="a995c-254">Přihlaste se k serveru vaší společnosti Bitbucket jako správce.</span><span class="sxs-lookup"><span data-stu-id="a995c-254">Log in to your Bitbucket company site as an administrator.</span></span>

2. <span data-ttu-id="a995c-255">Klikněte na ikonu nastavení.</span><span class="sxs-lookup"><span data-stu-id="a995c-255">Click on settings icon.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user1.png) 

3. <span data-ttu-id="a995c-257">V části **správy** oddíl, klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a995c-257">Under **Administration** tab section, click **Users**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user2.png)

4. <span data-ttu-id="a995c-259">Klikněte na tlačítko **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="a995c-259">Click **Create user**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user3.png)     

5. <span data-ttu-id="a995c-261">Na **vytvořit uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a995c-261">On the **Create User** dialog page, perform the following steps:</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user4.png) 

    <span data-ttu-id="a995c-263">a.</span><span class="sxs-lookup"><span data-stu-id="a995c-263">a.</span></span> <span data-ttu-id="a995c-264">V **uživatelské jméno** jako typ e-mailu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="a995c-264">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="a995c-265">b.</span><span class="sxs-lookup"><span data-stu-id="a995c-265">b.</span></span> <span data-ttu-id="a995c-266">V **úplný název** textovému poli, úplný název typu uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a995c-266">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>
    
    <span data-ttu-id="a995c-267">c.</span><span class="sxs-lookup"><span data-stu-id="a995c-267">c.</span></span> <span data-ttu-id="a995c-268">V **e-mailová adresa** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="a995c-268">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="a995c-269">d.</span><span class="sxs-lookup"><span data-stu-id="a995c-269">d.</span></span> <span data-ttu-id="a995c-270">V **heslo** textovému poli, zadejte heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="a995c-270">In the **Password** textbox, type the password of user.</span></span>  

    <span data-ttu-id="a995c-271">e.</span><span class="sxs-lookup"><span data-stu-id="a995c-271">e.</span></span> <span data-ttu-id="a995c-272">V **Potvrdit heslo** textovému poli, znovu zadejte heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="a995c-272">In the **Confirm Password** textbox, reenter the password of user.</span></span>

    <span data-ttu-id="a995c-273">f.</span><span class="sxs-lookup"><span data-stu-id="a995c-273">f.</span></span> <span data-ttu-id="a995c-274">Klikněte na tlačítko **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="a995c-274">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a995c-275">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a995c-275">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a995c-276">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí jednotného přihlašování k Kantega pro Bitbucket udělení přístupu.</span><span class="sxs-lookup"><span data-stu-id="a995c-276">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kantega SSO for Bitbucket.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a995c-278">**Pokud chcete přiřadit Britta Simon Kantega jednotné přihlašování pro Bitbucket, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a995c-278">**To assign Britta Simon to Kantega SSO for Bitbucket, perform the following steps:**</span></span>

1. <span data-ttu-id="a995c-279">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a995c-279">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a995c-281">V seznamu aplikací vyberte **Kantega jednotné přihlašování pro Bitbucket**.</span><span class="sxs-lookup"><span data-stu-id="a995c-281">In the applications list, select **Kantega SSO for Bitbucket**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_app.png) 

3. <span data-ttu-id="a995c-283">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a995c-283">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a995c-285">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a995c-285">Click **Add** button.</span></span> <span data-ttu-id="a995c-286">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a995c-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a995c-288">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a995c-288">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a995c-289">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a995c-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a995c-290">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a995c-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a995c-291">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a995c-291">Testing single sign-on</span></span>

<span data-ttu-id="a995c-292">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a995c-292">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a995c-293">Po kliknutí na tlačítko Kantega jednotné přihlašování pro dlaždici Bitbucket na přístupovém panelu, můžete by měl získat automaticky přihlášení k vaší Kantega jednotné přihlašování pro aplikace Bitbucket.</span><span class="sxs-lookup"><span data-stu-id="a995c-293">When you click the Kantega SSO for Bitbucket tile in the Access Panel, you should get automatically signed-on to your Kantega SSO for Bitbucket application.</span></span>
<span data-ttu-id="a995c-294">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a995c-294">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a995c-295">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a995c-295">Additional resources</span></span>

* [<span data-ttu-id="a995c-296">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a995c-296">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a995c-297">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a995c-297">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_203.png

