---
title: "Kurz: Azure Active Directory integrace s Mimecast osobní portál | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Mimecast osobní portál."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ee2a8edcab36f295732ac1ebe641ed7fcfc1f2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a><span data-ttu-id="fff60-103">Kurz: Azure Active Directory integrace s Mimecast osobní portálu</span><span class="sxs-lookup"><span data-stu-id="fff60-103">Tutorial: Azure Active Directory integration with Mimecast Personal Portal</span></span>

<span data-ttu-id="fff60-104">V tomto kurzu zjistíte, jak toointegrate Mimecast osobní portálu v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fff60-104">In this tutorial, you learn how toointegrate Mimecast Personal Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fff60-105">Integrace portálu osobní Mimecast s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="fff60-105">Integrating Mimecast Personal Portal with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fff60-106">Můžete řídit ve službě Azure AD, který má přístup tooMimecast osobní portálu</span><span class="sxs-lookup"><span data-stu-id="fff60-106">You can control in Azure AD who has access tooMimecast Personal Portal</span></span>
- <span data-ttu-id="fff60-107">Vaši uživatelé tooautomatically get přihlášeného tooMimecast osobní portálu (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="fff60-107">You can enable your users tooautomatically get signed-on tooMimecast Personal Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fff60-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fff60-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="fff60-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fff60-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fff60-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fff60-110">Prerequisites</span></span>

<span data-ttu-id="fff60-111">tooconfigure integrace Azure AD s Mimecast osobní portál, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="fff60-111">tooconfigure Azure AD integration with Mimecast Personal Portal, you need hello following items:</span></span>

- <span data-ttu-id="fff60-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fff60-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fff60-113">Portálu osobní Mimecast jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="fff60-113">A Mimecast Personal Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fff60-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fff60-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fff60-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="fff60-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fff60-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="fff60-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fff60-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fff60-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fff60-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="fff60-118">Scenario description</span></span>
<span data-ttu-id="fff60-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fff60-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fff60-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="fff60-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fff60-121">Přidání osobní portálu Mimecast z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="fff60-121">Adding Mimecast Personal Portal from hello gallery</span></span>
2. <span data-ttu-id="fff60-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fff60-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-personal-portal-from-hello-gallery"></a><span data-ttu-id="fff60-123">Přidání osobní portálu Mimecast z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="fff60-123">Adding Mimecast Personal Portal from hello gallery</span></span>
<span data-ttu-id="fff60-124">tooconfigure hello integrace Mimecast osobní portálu do Azure AD, je nutné tooadd Mimecast osobní portál hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="fff60-124">tooconfigure hello integration of Mimecast Personal Portal into Azure AD, you need tooadd Mimecast Personal Portal from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fff60-125">**tooadd Mimecast osobní portál z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fff60-125">**tooadd Mimecast Personal Portal from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fff60-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fff60-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fff60-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="fff60-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fff60-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fff60-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="fff60-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fff60-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="fff60-133">Hello vyhledávacího pole zadejte **Mimecast osobní portál**.</span><span class="sxs-lookup"><span data-stu-id="fff60-133">In hello search box, type **Mimecast Personal Portal**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. <span data-ttu-id="fff60-135">Na panelu výsledků hello vyberte **osobní portál Mimecast**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="fff60-135">In hello results panel, select **Mimecast Personal Portal**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fff60-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fff60-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fff60-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mimecast osobní portál podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="fff60-138">In this section, you configure and test Azure AD single sign-on with Mimecast Personal Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fff60-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello osobní portálu Mimecast je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fff60-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mimecast Personal Portal is tooa user in Azure AD.</span></span> <span data-ttu-id="fff60-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello osobní portálu Mimecast musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="fff60-140">In other words, a link relationship between an Azure AD user and hello related user in Mimecast Personal Portal needs toobe established.</span></span>

<span data-ttu-id="fff60-141">Osobní portálu Mimecast přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="fff60-141">In Mimecast Personal Portal, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fff60-142">tooconfigure a testu Azure AD jednotné přihlašování s Mimecast osobní portál, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="fff60-142">tooconfigure and test Azure AD single sign-on with Mimecast Personal Portal, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fff60-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="fff60-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fff60-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fff60-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fff60-145">**[Vytvoření zkušebního uživatele osobní portál Mimecast](#creating-a-mimecast-personal-portal-test-user)**  -toohave protějšek Britta Simon Mimecast osobní portálu, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="fff60-145">**[Creating a Mimecast Personal Portal test user](#creating-a-mimecast-personal-portal-test-user)** - toohave a counterpart of Britta Simon in Mimecast Personal Portal that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fff60-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fff60-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fff60-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="fff60-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fff60-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fff60-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fff60-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Mimecast osobní portál.</span><span class="sxs-lookup"><span data-stu-id="fff60-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mimecast Personal Portal application.</span></span>

<span data-ttu-id="fff60-150">**tooconfigure Azure AD jednotné přihlašování s Mimecast osobní portál, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fff60-150">**tooconfigure Azure AD single sign-on with Mimecast Personal Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="fff60-151">V portálu Azure, na hello hello **osobní portál Mimecast** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="fff60-151">In hello Azure portal, on hello **Mimecast Personal Portal** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="fff60-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fff60-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. <span data-ttu-id="fff60-155">Na hello **Mimecast osobní portál domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="fff60-155">On hello **Mimecast Personal Portal Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    <span data-ttu-id="fff60-157">a.</span><span class="sxs-lookup"><span data-stu-id="fff60-157">a.</span></span> <span data-ttu-id="fff60-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="fff60-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    <span data-ttu-id="fff60-159">b.</span><span class="sxs-lookup"><span data-stu-id="fff60-159">b.</span></span> <span data-ttu-id="fff60-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="fff60-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > <span data-ttu-id="fff60-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="fff60-161">These values are not real.</span></span> <span data-ttu-id="fff60-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="fff60-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="fff60-163">Obraťte se na [tým podpory Mimecast osobní portál klienta](https://www.mimecast.com/customer-success/technical-support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="fff60-163">Contact [Mimecast Personal Portal Client support team](https://www.mimecast.com/customer-success/technical-support/) tooget these values.</span></span> 
 


4. <span data-ttu-id="fff60-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="fff60-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. <span data-ttu-id="fff60-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fff60-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fff60-168">Na hello **osobní konfigurace portálu Mimecast** klikněte na tlačítko **nakonfigurovat portál osobní Mimecast** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="fff60-168">On hello **Mimecast Personal Portal Configuration** section, click **Configure Mimecast Personal Portal** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fff60-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="fff60-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. <span data-ttu-id="fff60-171">V okně prohlížeče jiný web Přihlaste se k portálu osobní Mimecast jako správce.</span><span class="sxs-lookup"><span data-stu-id="fff60-171">In a different web browser window, log into your Mimecast Personal Portal as an administrator.</span></span>

8. <span data-ttu-id="fff60-172">Přejděte příliš**služby \> aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fff60-172">Go too**Services \> Applications**.</span></span>
   
    <span data-ttu-id="fff60-173">![Aplikace](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="fff60-173">![Applications](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "Applications")</span></span>

9. <span data-ttu-id="fff60-174">Klikněte na tlačítko **ověřování profily**.</span><span class="sxs-lookup"><span data-stu-id="fff60-174">Click **Authentication Profiles**.</span></span>
   
    <span data-ttu-id="fff60-175">![Profily ověřování](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "profily ověřování")</span><span class="sxs-lookup"><span data-stu-id="fff60-175">![Authentication Profiles](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")</span></span>

10. <span data-ttu-id="fff60-176">Klikněte na tlačítko **nový profil ověřování**.</span><span class="sxs-lookup"><span data-stu-id="fff60-176">Click **New Authentication Profile**.</span></span>
   
    <span data-ttu-id="fff60-177">![Nový profil ověřování](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "nový profil ověřování")</span><span class="sxs-lookup"><span data-stu-id="fff60-177">![New Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "New Authentication Profile")</span></span>

11. <span data-ttu-id="fff60-178">V hello **profil ověření** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="fff60-178">In hello **Authentication Profile** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="fff60-179">![Profil ověření](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "profil ověření")</span><span class="sxs-lookup"><span data-stu-id="fff60-179">![Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")</span></span>
   
    <span data-ttu-id="fff60-180">a.</span><span class="sxs-lookup"><span data-stu-id="fff60-180">a.</span></span> <span data-ttu-id="fff60-181">V hello **popis** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fff60-181">In hello **Description** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="fff60-182">b.</span><span class="sxs-lookup"><span data-stu-id="fff60-182">b.</span></span> <span data-ttu-id="fff60-183">Vyberte **vynutit ověřování SAML pro portál Mimecast osobní**.</span><span class="sxs-lookup"><span data-stu-id="fff60-183">Select **Enforce SAML Authentication for Mimecast Personal Portal**.</span></span>
   
    <span data-ttu-id="fff60-184">c.</span><span class="sxs-lookup"><span data-stu-id="fff60-184">c.</span></span> <span data-ttu-id="fff60-185">Jako **zprostředkovatele**, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fff60-185">As **Provider**, select **Azure Active Directory**.</span></span>
   
    <span data-ttu-id="fff60-186">d.</span><span class="sxs-lookup"><span data-stu-id="fff60-186">d.</span></span> <span data-ttu-id="fff60-187">V **URL vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fff60-187">In **Issuer URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="fff60-188">e.</span><span class="sxs-lookup"><span data-stu-id="fff60-188">e.</span></span> <span data-ttu-id="fff60-189">V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fff60-189">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="fff60-190">f.</span><span class="sxs-lookup"><span data-stu-id="fff60-190">f.</span></span> <span data-ttu-id="fff60-191">V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fff60-191">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="fff60-192">g.</span><span class="sxs-lookup"><span data-stu-id="fff60-192">g.</span></span> <span data-ttu-id="fff60-193">Otevřete váš **kódování base-64** kódování certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity (Metadata)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="fff60-193">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate (Metadata)** textbox.</span></span>

    <span data-ttu-id="fff60-194">h.</span><span class="sxs-lookup"><span data-stu-id="fff60-194">h.</span></span> <span data-ttu-id="fff60-195">Vyberte **povolit jednotné přihlašování na**.</span><span class="sxs-lookup"><span data-stu-id="fff60-195">Select **Allow Single Sign On**.</span></span>
   
    <span data-ttu-id="fff60-196">i.</span><span class="sxs-lookup"><span data-stu-id="fff60-196">i.</span></span> <span data-ttu-id="fff60-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fff60-197">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="fff60-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="fff60-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fff60-199">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="fff60-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fff60-200">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fff60-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fff60-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fff60-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="fff60-202">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="fff60-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="fff60-204">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fff60-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fff60-205">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fff60-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fff60-207">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fff60-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fff60-209">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="fff60-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fff60-211">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fff60-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fff60-213">a.</span><span class="sxs-lookup"><span data-stu-id="fff60-213">a.</span></span> <span data-ttu-id="fff60-214">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fff60-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fff60-215">b.</span><span class="sxs-lookup"><span data-stu-id="fff60-215">b.</span></span> <span data-ttu-id="fff60-216">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fff60-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fff60-217">c.</span><span class="sxs-lookup"><span data-stu-id="fff60-217">c.</span></span> <span data-ttu-id="fff60-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="fff60-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="fff60-219">d.</span><span class="sxs-lookup"><span data-stu-id="fff60-219">d.</span></span> <span data-ttu-id="fff60-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fff60-220">Click **Create**.</span></span>
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a><span data-ttu-id="fff60-221">Vytvoření zkušebního uživatele Mimecast osobní portálu</span><span class="sxs-lookup"><span data-stu-id="fff60-221">Creating a Mimecast Personal Portal test user</span></span>

<span data-ttu-id="fff60-222">V pořadí tooenable Azure AD Uživatelé toolog Mimecast osobní portálu musí být zřízená Mimecast osobní portálu.</span><span class="sxs-lookup"><span data-stu-id="fff60-222">In order tooenable Azure AD users toolog into Mimecast Personal Portal, they must be provisioned into Mimecast Personal Portal.</span></span> <span data-ttu-id="fff60-223">V případě hello osobní portálu Mimecast zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="fff60-223">In hello case of Mimecast Personal Portal, provisioning is a manual task.</span></span>

<span data-ttu-id="fff60-224">Než můžete vytvořit uživatele, musíte tooregister domény.</span><span class="sxs-lookup"><span data-stu-id="fff60-224">You need tooregister a domain before you can create users.</span></span>

<span data-ttu-id="fff60-225">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fff60-225">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="fff60-226">Přihlaste se tooyour **Mimecast osobní portál** jako správce.</span><span class="sxs-lookup"><span data-stu-id="fff60-226">Sign on tooyour **Mimecast Personal Portal** as administrator.</span></span>

2. <span data-ttu-id="fff60-227">Přejděte příliš**adresáře \> interní**.</span><span class="sxs-lookup"><span data-stu-id="fff60-227">Go too**Directories \> Internal**.</span></span>
   
    <span data-ttu-id="fff60-228">![Adresáře](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "adresáře")</span><span class="sxs-lookup"><span data-stu-id="fff60-228">![Directories](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "Directories")</span></span>

3. <span data-ttu-id="fff60-229">Klikněte na tlačítko **zaregistrovat nové domény**.</span><span class="sxs-lookup"><span data-stu-id="fff60-229">Click **Register New Domain**.</span></span>
   
    <span data-ttu-id="fff60-230">![Zaregistrujte novou doménu](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "zaregistrovat nové domény")</span><span class="sxs-lookup"><span data-stu-id="fff60-230">![Register New Domain](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")</span></span>

4. <span data-ttu-id="fff60-231">Po vytvoření nové domény, klikněte na tlačítko **novou adresu**.</span><span class="sxs-lookup"><span data-stu-id="fff60-231">After your new domain has been created, click **New Address**.</span></span>
   
    <span data-ttu-id="fff60-232">![Nové adresy](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "novou adresu")</span><span class="sxs-lookup"><span data-stu-id="fff60-232">![New Address](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "New Address")</span></span>

5. <span data-ttu-id="fff60-233">V hello adresu dialogové okno Nový, provádět hello kroků platný Azure AD účet chcete tooprovision:</span><span class="sxs-lookup"><span data-stu-id="fff60-233">In hello new address dialog, perform hello following steps of a valid Azure AD account you want tooprovision:</span></span>
   
    <span data-ttu-id="fff60-234">![Uložit](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "uložit")</span><span class="sxs-lookup"><span data-stu-id="fff60-234">![Save](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "Save")</span></span>
   
    <span data-ttu-id="fff60-235">a.</span><span class="sxs-lookup"><span data-stu-id="fff60-235">a.</span></span> <span data-ttu-id="fff60-236">V hello **e-mailovou adresu** textovému poli, typ **e-mailovou adresu** hello uživatele jako  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="fff60-236">In hello **Email Address** textbox, type **Email Address** of hello user as **BrittaSimon@contoso.com**.</span></span>
    
    <span data-ttu-id="fff60-237">b.</span><span class="sxs-lookup"><span data-stu-id="fff60-237">b.</span></span> <span data-ttu-id="fff60-238">V hello **globální název** textovému poli, typ hello **uživatelské jméno** jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fff60-238">In hello **Global Name** textbox, type hello **username** as **BrittaSimon**.</span></span>

    <span data-ttu-id="fff60-239">c.</span><span class="sxs-lookup"><span data-stu-id="fff60-239">c.</span></span> <span data-ttu-id="fff60-240">V hello **heslo**, a **Potvrdit heslo** textová pole, typ hello **heslo** hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="fff60-240">In hello **Password**, and **Confirm Password** textboxes, type hello **Password** of hello user.</span></span>
   
    <span data-ttu-id="fff60-241">b.</span><span class="sxs-lookup"><span data-stu-id="fff60-241">b.</span></span> <span data-ttu-id="fff60-242">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fff60-242">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="fff60-243">Můžete použít jiné nástroje pro tvorbu účet uživatele Mimecast osobní portál nebo rozhraní API poskytovaných osobní portál Mimecast tooprovision Azure AD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="fff60-243">You can use any other Mimecast Personal Portal user account creation tools or APIs provided by Mimecast Personal Portal tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="fff60-244">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="fff60-244">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="fff60-245">V této části povolíte tak, že udělíte přístup tooMimecast osobní portál Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fff60-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMimecast Personal Portal.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="fff60-247">**tooassign Britta Simon tooMimecast osobní portál, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fff60-247">**tooassign Britta Simon tooMimecast Personal Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="fff60-248">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fff60-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="fff60-250">V seznamu aplikace hello vyberte **Mimecast osobní portál**.</span><span class="sxs-lookup"><span data-stu-id="fff60-250">In hello applications list, select **Mimecast Personal Portal**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. <span data-ttu-id="fff60-252">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="fff60-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="fff60-254">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fff60-254">Click **Add** button.</span></span> <span data-ttu-id="fff60-255">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fff60-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="fff60-257">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="fff60-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fff60-258">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fff60-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fff60-259">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fff60-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fff60-260">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fff60-260">Testing single sign-on</span></span>
<span data-ttu-id="fff60-261">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="fff60-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="fff60-262">Když kliknete na dlaždici osobní portál Mimecast hello v hello přístupového panelu, měli byste obdržet aplikace automaticky přihlášeného tooyour Mimecast osobní portálu.</span><span class="sxs-lookup"><span data-stu-id="fff60-262">When you click hello Mimecast Personal Portal tile in hello Access Panel, you should get automatically signed-on tooyour Mimecast Personal Portal application.</span></span> <span data-ttu-id="fff60-263">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fff60-263">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fff60-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fff60-264">Additional resources</span></span>

* [<span data-ttu-id="fff60-265">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fff60-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fff60-266">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fff60-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

