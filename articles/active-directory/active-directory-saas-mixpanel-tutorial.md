---
title: 'Kurz: Azure Active Directory integrace s Mixpanel | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Mixpanel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 8da8aaefee3558c37babe3e10aeba4224ceffa16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a><span data-ttu-id="d2d98-103">Kurz: Azure Active Directory integrace s Mixpanel</span><span class="sxs-lookup"><span data-stu-id="d2d98-103">Tutorial: Azure Active Directory integration with Mixpanel</span></span>

<span data-ttu-id="d2d98-104">V tomto kurzu zjistíte, jak toointegrate Mixpanel s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d2d98-104">In this tutorial, you learn how toointegrate Mixpanel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d2d98-105">Integrace Mixpanel s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d2d98-105">Integrating Mixpanel with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d2d98-106">Můžete řídit ve službě Azure AD, který má přístup tooMixpanel</span><span class="sxs-lookup"><span data-stu-id="d2d98-106">You can control in Azure AD who has access tooMixpanel</span></span>
- <span data-ttu-id="d2d98-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMixpanel (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2d98-107">You can enable your users tooautomatically get signed-on tooMixpanel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d2d98-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d2d98-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d2d98-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d2d98-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2d98-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2d98-110">Prerequisites</span></span>

<span data-ttu-id="d2d98-111">Integrace služby Azure AD s Mixpanel tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d2d98-111">tooconfigure Azure AD integration with Mixpanel, you need hello following items:</span></span>

- <span data-ttu-id="d2d98-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2d98-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d2d98-113">Mixpanel jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d2d98-113">A Mixpanel single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d2d98-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d2d98-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d2d98-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d2d98-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d2d98-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d2d98-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d2d98-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d2d98-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d2d98-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d2d98-118">Scenario description</span></span>
<span data-ttu-id="d2d98-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d2d98-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d2d98-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d2d98-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d2d98-121">Přidání Mixpanel z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d2d98-121">Adding Mixpanel from hello gallery</span></span>
2. <span data-ttu-id="d2d98-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2d98-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mixpanel-from-hello-gallery"></a><span data-ttu-id="d2d98-123">Přidání Mixpanel z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d2d98-123">Adding Mixpanel from hello gallery</span></span>
<span data-ttu-id="d2d98-124">tooconfigure hello integrace Mixpanel do Azure AD, je nutné tooadd Mixpanel hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d2d98-124">tooconfigure hello integration of Mixpanel into Azure AD, you need tooadd Mixpanel from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d2d98-125">**tooadd Mixpanel z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d2d98-125">**tooadd Mixpanel from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2d98-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d2d98-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d2d98-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d2d98-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d2d98-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2d98-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d2d98-133">Hello vyhledávacího pole zadejte **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-133">In hello search box, type **Mixpanel**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_search.png)

5. <span data-ttu-id="d2d98-135">Na panelu výsledků hello vyberte **Mixpanel**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d2d98-135">In hello results panel, select **Mixpanel**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d2d98-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2d98-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d2d98-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mixpanel podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d2d98-138">In this section, you configure and test Azure AD single sign-on with Mixpanel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d2d98-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Mixpanel je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2d98-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mixpanel is tooa user in Azure AD.</span></span> <span data-ttu-id="d2d98-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Mixpanel musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d2d98-140">In other words, a link relationship between an Azure AD user and hello related user in Mixpanel needs toobe established.</span></span>

<span data-ttu-id="d2d98-141">V Mixpanel, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d2d98-141">In Mixpanel, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d2d98-142">tooconfigure a testu Azure AD jednotné přihlašování s Mixpanel, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d2d98-142">tooconfigure and test Azure AD single sign-on with Mixpanel, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d2d98-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d2d98-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d2d98-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d2d98-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d2d98-145">**[Vytvoření zkušebního uživatele Mixpanel](#creating-a-mixpanel-test-user)**  -toohave protějšek Britta Simon v Mixpanel, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d2d98-145">**[Creating a Mixpanel test user](#creating-a-mixpanel-test-user)** - toohave a counterpart of Britta Simon in Mixpanel that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d2d98-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d2d98-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d2d98-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d2d98-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d2d98-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2d98-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d2d98-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="d2d98-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mixpanel application.</span></span>

<span data-ttu-id="d2d98-150">**tooconfigure Azure AD jednotné přihlašování s Mixpanel, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d2d98-150">**tooconfigure Azure AD single sign-on with Mixpanel, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2d98-151">V portálu Azure, na hello hello **Mixpanel** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-151">In hello Azure portal, on hello **Mixpanel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d2d98-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d2d98-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

3. <span data-ttu-id="d2d98-155">Na hello **Mixpanel domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d2d98-155">On hello **Mixpanel Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_url.png)

     <span data-ttu-id="d2d98-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://mixpanel.com/login/`</span><span class="sxs-lookup"><span data-stu-id="d2d98-157">In hello **Sign-on URL** textbox, type a URL as: `https://mixpanel.com/login/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d2d98-158">Zaregistrujte se prosím na [https://mixpanel.com/register/](https://mixpanel.com/register/) tooset přihlašovací údaje a kontaktní hello [tým podpory Mixpanel](mailto:support@mixpanel.com) tooenable nastavení jednotného přihlašování pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="d2d98-158">Please register at [https://mixpanel.com/register/](https://mixpanel.com/register/) tooset up your login credentials and  contact hello [Mixpanel support team](mailto:support@mixpanel.com) tooenable SSO settings for your tenant.</span></span> <span data-ttu-id="d2d98-159">Hodnota přihlašovací adresa URL můžete také získat v případě potřeby váš tým podpory Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="d2d98-159">You can also get your Sign On URL value if necessary from your Mixpanel support team.</span></span> 
 
4. <span data-ttu-id="d2d98-160">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d2d98-160">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

5. <span data-ttu-id="d2d98-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2d98-162">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d2d98-164">Na hello **Mixpanel konfigurace** klikněte na tlačítko **konfigurace Mixpanel** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d2d98-164">On hello **Mixpanel Configuration** section, click **Configure Mixpanel** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d2d98-165">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d2d98-165">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_configure.png) 

7. <span data-ttu-id="d2d98-167">V okně jiný prohlížeč, přihlášení tooyour Mixpanel aplikace jako správce.</span><span class="sxs-lookup"><span data-stu-id="d2d98-167">In a different browser window, sign-on tooyour Mixpanel application as an administrator.</span></span>

8. <span data-ttu-id="d2d98-168">V dolní části stránky hello, klikněte na tlačítko hello trochu **zařízení** ikonu v levém horním hello.</span><span class="sxs-lookup"><span data-stu-id="d2d98-168">On bottom of hello page, click hello little **gear** icon in hello left corner.</span></span> 
   
    ![Mixpanel jednotné přihlášení](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

9. <span data-ttu-id="d2d98-170">Klikněte na tlačítko hello **zabezpečení přístupu k** a pak klikněte **změnit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-170">Click hello **Access security** tab, and then click **Change settings**.</span></span>
   
    ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 

10. <span data-ttu-id="d2d98-172">Na hello **změnit svůj certifikát** dialogové okno stránky, klikněte na tlačítko **zvolte soubor** tooupload stažený certifikát a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-172">On hello **Change your certificate** dialog page, click **Choose file** tooupload your downloaded certificate, and then click **NEXT**.</span></span>
   
    ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

11.  <span data-ttu-id="d2d98-174">V textovém poli Adresa URL ověřování hello na hello **změnit adresu URL ověřování** dialogové okno stránky, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-174">In hello authentication URL textbox on hello **Change your authentication  URL** dialog page, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal, and then click **NEXT**.</span></span>
   
   ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 

12. <span data-ttu-id="d2d98-176">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="d2d98-176">Click **Done**.</span></span>

> [!TIP]
> <span data-ttu-id="d2d98-177">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d2d98-177">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d2d98-178">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d2d98-178">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d2d98-179">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d2d98-179">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d2d98-180">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2d98-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="d2d98-181">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d2d98-181">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d2d98-183">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d2d98-183">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2d98-184">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d2d98-184">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d2d98-186">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-186">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d2d98-188">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="d2d98-188">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d2d98-190">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d2d98-190">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d2d98-192">a.</span><span class="sxs-lookup"><span data-stu-id="d2d98-192">a.</span></span> <span data-ttu-id="d2d98-193">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-193">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d2d98-194">b.</span><span class="sxs-lookup"><span data-stu-id="d2d98-194">b.</span></span> <span data-ttu-id="d2d98-195">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d2d98-195">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d2d98-196">c.</span><span class="sxs-lookup"><span data-stu-id="d2d98-196">c.</span></span> <span data-ttu-id="d2d98-197">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-197">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d2d98-198">d.</span><span class="sxs-lookup"><span data-stu-id="d2d98-198">d.</span></span> <span data-ttu-id="d2d98-199">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-199">Click **Create**.</span></span>
 
### <a name="creating-a-mixpanel-test-user"></a><span data-ttu-id="d2d98-200">Vytvoření zkušebního uživatele Mixpanel</span><span class="sxs-lookup"><span data-stu-id="d2d98-200">Creating a Mixpanel test user</span></span>

<span data-ttu-id="d2d98-201">Hello cílem této části je toocreate volal Britta Simon v Mixpanel uživatele.</span><span class="sxs-lookup"><span data-stu-id="d2d98-201">hello objective of this section is toocreate a user called Britta Simon in Mixpanel.</span></span> 

1. <span data-ttu-id="d2d98-202">Přihlaste se na tooyour Mixpanel společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="d2d98-202">Sign on tooyour Mixpanel company site as an administrator.</span></span>

2. <span data-ttu-id="d2d98-203">Na hello dolní části stránky hello, klikněte na tlačítko málo ozubené kolečko hello rohu tooopen hello hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="d2d98-203">On hello bottom of hello page, click hello little gear button on hello left corner tooopen hello **Settings** window.</span></span>

3. <span data-ttu-id="d2d98-204">Klikněte na tlačítko hello **Team** kartě.</span><span class="sxs-lookup"><span data-stu-id="d2d98-204">Click hello **Team** tab.</span></span>

4. <span data-ttu-id="d2d98-205">V hello **člen týmu** textovému poli, zadejte e-mailovou adresu na Britta hello Azure.</span><span class="sxs-lookup"><span data-stu-id="d2d98-205">In hello **team member** textbox, type Britta's email address in hello Azure.</span></span>
   
    ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. <span data-ttu-id="d2d98-207">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-207">Click **Invite**.</span></span> 

> [!Note]
> <span data-ttu-id="d2d98-208">Hello uživatel dostane e-mailu tooset hello profil.</span><span class="sxs-lookup"><span data-stu-id="d2d98-208">hello user will get an email tooset up hello profile.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d2d98-209">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d2d98-209">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d2d98-210">V této části povolíte tak, že udělíte přístup tooMixpanel toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d2d98-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMixpanel.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d2d98-212">**tooassign Britta Simon tooMixpanel, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d2d98-212">**tooassign Britta Simon tooMixpanel, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2d98-213">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d2d98-215">V seznamu aplikace hello vyberte **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-215">In hello applications list, select **Mixpanel**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_app.png) 

3. <span data-ttu-id="d2d98-217">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d2d98-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d2d98-219">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2d98-219">Click **Add** button.</span></span> <span data-ttu-id="d2d98-220">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2d98-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d2d98-222">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d2d98-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d2d98-223">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2d98-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d2d98-224">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2d98-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d2d98-225">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2d98-225">Testing single sign-on</span></span>

<span data-ttu-id="d2d98-226">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d2d98-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d2d98-227">Když kliknete na dlaždici Mixpanel hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Mixpanel aplikace.</span><span class="sxs-lookup"><span data-stu-id="d2d98-227">When you click hello Mixpanel tile in hello Access Panel, you should get automatically signed-on tooyour Mixpanel application.</span></span>
<span data-ttu-id="d2d98-228">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d2d98-228">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d2d98-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d2d98-229">Additional resources</span></span>

* [<span data-ttu-id="d2d98-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d2d98-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d2d98-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d2d98-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png

