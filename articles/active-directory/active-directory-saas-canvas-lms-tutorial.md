---
title: "Kurz: Azure Active Directory integrace s plátno pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a plátno pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 8f4a09266a108e2c92326b0909dd0650b1c84d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a><span data-ttu-id="b1c8c-103">Kurz: Azure Active Directory integrace s plátno pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="b1c8c-103">Tutorial: Azure Active Directory integration with Canvas LMS</span></span>

<span data-ttu-id="b1c8c-104">V tomto kurzu zjistíte, jak toointegrate plátno s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b1c8c-104">In this tutorial, you learn how toointegrate Canvas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1c8c-105">Integrace plátno s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-105">Integrating Canvas with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b1c8c-106">Můžete řídit ve službě Azure AD, který má přístup tooCanvas</span><span class="sxs-lookup"><span data-stu-id="b1c8c-106">You can control in Azure AD who has access tooCanvas</span></span>
- <span data-ttu-id="b1c8c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCanvas (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1c8c-107">You can enable your users tooautomatically get signed-on tooCanvas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1c8c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b1c8c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b1c8c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1c8c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1c8c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b1c8c-110">Prerequisites</span></span>

<span data-ttu-id="b1c8c-111">Integrace služby Azure AD plátno tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-111">tooconfigure Azure AD integration with Canvas, you need hello following items:</span></span>

- <span data-ttu-id="b1c8c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1c8c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1c8c-113">Plátně jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b1c8c-113">A Canvas single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1c8c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1c8c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1c8c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b1c8c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1c8c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b1c8c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b1c8c-118">Scenario description</span></span>
<span data-ttu-id="b1c8c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1c8c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1c8c-121">Přidání plátno z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1c8c-121">Adding Canvas from hello gallery</span></span>
2. <span data-ttu-id="b1c8c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1c8c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-canvas-from-hello-gallery"></a><span data-ttu-id="b1c8c-123">Přidání plátno z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b1c8c-123">Adding Canvas from hello gallery</span></span>
<span data-ttu-id="b1c8c-124">tooconfigure hello integrace plátno do Azure AD, je nutné tooadd plátno hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-124">tooconfigure hello integration of Canvas into Azure AD, you need tooadd Canvas from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b1c8c-125">**tooadd plátno z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1c8c-125">**tooadd Canvas from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1c8c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b1c8c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b1c8c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b1c8c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b1c8c-133">Hello vyhledávacího pole zadejte **plátno**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-133">In hello search box, type **Canvas**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. <span data-ttu-id="b1c8c-135">Na panelu výsledků hello vyberte **plátno**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-135">In hello results panel, select **Canvas**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b1c8c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1c8c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b1c8c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování plátno podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b1c8c-138">In this section, you configure and test Azure AD single sign-on with Canvas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b1c8c-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello plátno je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Canvas is tooa user in Azure AD.</span></span> <span data-ttu-id="b1c8c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello plátno musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-140">In other words, a link relationship between an Azure AD user and hello related user in Canvas needs toobe established.</span></span>

<span data-ttu-id="b1c8c-141">V plátně přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-141">In Canvas, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b1c8c-142">tooconfigure a testu Azure AD jednotné přihlašování s plátno, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-142">tooconfigure and test Azure AD single sign-on with Canvas, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b1c8c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b1c8c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1c8c-145">**[Vytvoření zkušebního uživatele plátno](#creating-a-canvas-test-user)**  -toohave protějšek Britta Simon plátno, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-145">**[Creating a Canvas test user](#creating-a-canvas-test-user)** - toohave a counterpart of Britta Simon in Canvas that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b1c8c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b1c8c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b1c8c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1c8c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b1c8c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci plátno.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Canvas application.</span></span>

<span data-ttu-id="b1c8c-150">**tooconfigure Azure AD jednotné přihlašování plátno, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1c8c-150">**tooconfigure Azure AD single sign-on with Canvas, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1c8c-151">V portálu Azure, na hello hello **plátno** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-151">In hello Azure portal, on hello **Canvas** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b1c8c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. <span data-ttu-id="b1c8c-155">Na hello **plátno domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-155">On hello **Canvas Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    <span data-ttu-id="b1c8c-157">a.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-157">a.</span></span> <span data-ttu-id="b1c8c-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.instructure.com`</span><span class="sxs-lookup"><span data-stu-id="b1c8c-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.instructure.com`</span></span>

    <span data-ttu-id="b1c8c-159">b.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-159">b.</span></span> <span data-ttu-id="b1c8c-160">V hello **identifikátor** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<tenant-name>.instructure.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="b1c8c-160">In hello **Identifier** textbox, type hello value using hello following pattern: `https://<tenant-name>.instructure.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b1c8c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-161">These values are not real.</span></span> <span data-ttu-id="b1c8c-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b1c8c-163">Obraťte se na [tým podpory plátno klienta](https://community.canvaslms.com/community/help) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-163">Contact [Canvas Client support team](https://community.canvaslms.com/community/help) tooget these values.</span></span> 
 
4. <span data-ttu-id="b1c8c-164">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. <span data-ttu-id="b1c8c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b1c8c-168">Na hello **plátno konfigurace** klikněte na tlačítko **konfigurace plátno** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-168">On hello **Canvas Configuration** section, click **Configure Canvas** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b1c8c-169">Kopírování hello **heslo změnit adresu URL, adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b1c8c-169">Copy hello **Change Password URL, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. <span data-ttu-id="b1c8c-171">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour plátno.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-171">In a different web browser window, log in tooyour Canvas company site as an administrator.</span></span>

8. <span data-ttu-id="b1c8c-172">Přejděte příliš**kurzy \> účty spravované \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-172">Go too**Courses \> Managed Accounts \> Microsoft**.</span></span>
   
    <span data-ttu-id="b1c8c-173">![Plátno](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "plátno")</span><span class="sxs-lookup"><span data-stu-id="b1c8c-173">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

9. <span data-ttu-id="b1c8c-174">V navigačním podokně hello na levé straně hello vyberte **ověřování**a potom klikněte na **přidat nová konfigurace SAML**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-174">In hello navigation pane on hello left, select **Authentication**, and then click **Add New SAML Config**.</span></span>
   
    <span data-ttu-id="b1c8c-175">![Ověřování](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="b1c8c-175">![Authentication](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentication")</span></span>

10. <span data-ttu-id="b1c8c-176">Na stránce aktuální integrace hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-176">On hello Current Integration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="b1c8c-177">![Aktuální integrace](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuální integrace")</span><span class="sxs-lookup"><span data-stu-id="b1c8c-177">![Current Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Current Integration")</span></span>

    <span data-ttu-id="b1c8c-178">a.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-178">a.</span></span> <span data-ttu-id="b1c8c-179">V **IdP Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-179">In **IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b1c8c-180">b.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-180">b.</span></span> <span data-ttu-id="b1c8c-181">V **protokolu na adresu URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-181">In **Log On URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal .</span></span>

    <span data-ttu-id="b1c8c-182">c.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-182">c.</span></span> <span data-ttu-id="b1c8c-183">V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-183">In **Log Out URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b1c8c-184">d.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-184">d.</span></span> <span data-ttu-id="b1c8c-185">V **odkaz změnit heslo** textovému poli, vložte hodnotu hello **heslo změnit adresu URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-185">In **Change Password Link** textbox, paste hello value of **Change Password URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="b1c8c-186">e.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-186">e.</span></span> <span data-ttu-id="b1c8c-187">V **otisků prstů certifikátů** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-187">In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>      
        
    <span data-ttu-id="b1c8c-188">f.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-188">f.</span></span> <span data-ttu-id="b1c8c-189">Z hello **přihlášení atribut** seznamu, vyberte **NameID**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-189">From hello **Login Attribute** list, select **NameID**.</span></span>

    <span data-ttu-id="b1c8c-190">g.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-190">g.</span></span> <span data-ttu-id="b1c8c-191">Z hello **identifikátor formátu** seznamu, vyberte **emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-191">From hello **Identifier Format** list, select **emailAddress**.</span></span>

    <span data-ttu-id="b1c8c-192">h.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-192">h.</span></span> <span data-ttu-id="b1c8c-193">Klikněte na tlačítko **uložit nastavení ověřování**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-193">Click **Save Authentication Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="b1c8c-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b1c8c-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b1c8c-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b1c8c-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b1c8c-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b1c8c-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1c8c-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="b1c8c-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b1c8c-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1c8c-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1c8c-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b1c8c-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1c8c-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1c8c-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1c8c-209">a.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-209">a.</span></span> <span data-ttu-id="b1c8c-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1c8c-211">b.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-211">b.</span></span> <span data-ttu-id="b1c8c-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1c8c-213">c.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-213">c.</span></span> <span data-ttu-id="b1c8c-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b1c8c-215">d.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-215">d.</span></span> <span data-ttu-id="b1c8c-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-216">Click **Create**.</span></span>
 
### <a name="creating-a-canvas-test-user"></a><span data-ttu-id="b1c8c-217">Vytvoření zkušebního uživatele plátno</span><span class="sxs-lookup"><span data-stu-id="b1c8c-217">Creating a Canvas test user</span></span>

<span data-ttu-id="b1c8c-218">Uživatelé toolog tooenable Azure AD v tooCanvas, se musí být zřízená plátno.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-218">tooenable Azure AD users toolog in tooCanvas, they must be provisioned into Canvas.</span></span>

<span data-ttu-id="b1c8c-219">V případě plátně zřizování uživatelů je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-219">In case of Canvas, user provisioning is a manual task.</span></span>

<span data-ttu-id="b1c8c-220">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1c8c-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1c8c-221">Přihlaste se tooyour **plátno** klienta.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-221">Log in tooyour **Canvas** tenant.</span></span>

2. <span data-ttu-id="b1c8c-222">Přejděte příliš**kurzy \> účty spravované \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-222">Go too**Courses \> Managed Accounts \> Microsoft**.</span></span>
   
   <span data-ttu-id="b1c8c-223">![Plátno](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "plátno")</span><span class="sxs-lookup"><span data-stu-id="b1c8c-223">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

3. <span data-ttu-id="b1c8c-224">Klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-224">Click **Users**.</span></span>
   
   <span data-ttu-id="b1c8c-225">![Uživatelé](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b1c8c-225">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Users")</span></span>

4. <span data-ttu-id="b1c8c-226">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-226">Click **Add New User**.</span></span>
   
   <span data-ttu-id="b1c8c-227">![Uživatelé](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b1c8c-227">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Users")</span></span>

5. <span data-ttu-id="b1c8c-228">V hello přidat nového uživatele dialogové okno stránky proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b1c8c-228">On hello Add a New User dialog page, perform hello following steps:</span></span>
   
   <span data-ttu-id="b1c8c-229">![Přidat uživatele](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="b1c8c-229">![Add User](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Add User")</span></span>
   
   <span data-ttu-id="b1c8c-230">a.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-230">a.</span></span> <span data-ttu-id="b1c8c-231">V hello **úplný název** textovému poli, zadejte název hello uživatele jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-231">In hello **Full Name** textbox, enter hello name of user like **BrittaSimon**.</span></span>

   <span data-ttu-id="b1c8c-232">b.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-232">b.</span></span> <span data-ttu-id="b1c8c-233">V hello **e-mailu** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="b1c8c-233">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="b1c8c-234">c.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-234">c.</span></span> <span data-ttu-id="b1c8c-235">V hello **přihlášení** textovému poli, zadejte hello uživatele Azure AD e-mailovou adresu jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="b1c8c-235">In hello **Login** textbox, enter hello user’s Azure AD email address like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="b1c8c-236">d.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-236">d.</span></span> <span data-ttu-id="b1c8c-237">Vyberte **e-mailem hello uživatel o vytvoření tohoto účtu**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-237">Select **Email hello user about this account creation**.</span></span>

   <span data-ttu-id="b1c8c-238">e.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-238">e.</span></span> <span data-ttu-id="b1c8c-239">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-239">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="b1c8c-240">Můžete použít všechny ostatní plátno uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované plátno tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-240">You can use any other Canvas user account creation tools or APIs provided by Canvas tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b1c8c-241">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b1c8c-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b1c8c-242">V této části povolíte tak, že udělíte přístup tooCanvas toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCanvas.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b1c8c-244">**tooassign Britta Simon tooCanvas, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b1c8c-244">**tooassign Britta Simon tooCanvas, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1c8c-245">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b1c8c-247">V seznamu aplikace hello vyberte **plátno**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-247">In hello applications list, select **Canvas**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. <span data-ttu-id="b1c8c-249">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b1c8c-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-251">Click **Add** button.</span></span> <span data-ttu-id="b1c8c-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b1c8c-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b1c8c-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1c8c-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b1c8c-257">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b1c8c-257">Testing single sign-on</span></span>

<span data-ttu-id="b1c8c-258">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b1c8c-259">Když kliknete na dlaždici plátno hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour plátno aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1c8c-259">When you click hello Canvas tile in hello Access Panel, you should get automatically signed-on tooyour Canvas application.</span></span>
<span data-ttu-id="b1c8c-260">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b1c8c-260">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1c8c-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b1c8c-261">Additional resources</span></span>

* [<span data-ttu-id="b1c8c-262">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b1c8c-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1c8c-263">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b1c8c-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

