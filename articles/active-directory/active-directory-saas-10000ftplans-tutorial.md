---
title: "Kurz: Azure Active Directory integrace s 10 000 ft plány | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a 10 000 ft plány."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 9aa6fd079da4f931d4dd7277407a07e56091d7c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a><span data-ttu-id="d0abc-103">Kurz: Azure Active Directory integrace s 10 000 ft plány</span><span class="sxs-lookup"><span data-stu-id="d0abc-103">Tutorial: Azure Active Directory integration with 10,000ft Plans</span></span>

<span data-ttu-id="d0abc-104">V tomto kurzu zjistíte, jak toointegrate 10 000 ft plány s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d0abc-104">In this tutorial, you learn how toointegrate 10,000ft Plans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d0abc-105">Integrace 10 000 ft plány s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d0abc-105">Integrating 10,000ft Plans with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d0abc-106">Můžete řídit ve službě Azure AD, který má přístup too10, plány 000ft</span><span class="sxs-lookup"><span data-stu-id="d0abc-106">You can control in Azure AD who has access too10,000ft Plans</span></span>
- <span data-ttu-id="d0abc-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného too10, plány 000ft (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d0abc-107">You can enable your users tooautomatically get signed-on too10,000ft Plans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d0abc-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d0abc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d0abc-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d0abc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0abc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d0abc-110">Prerequisites</span></span>

<span data-ttu-id="d0abc-111">tooconfigure integrace Azure AD s 10 000 ft plány, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d0abc-111">tooconfigure Azure AD integration with 10,000ft Plans, you need hello following items:</span></span>

- <span data-ttu-id="d0abc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d0abc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d0abc-113">10 000 ft plány jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d0abc-113">A 10,000ft Plans single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d0abc-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d0abc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d0abc-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d0abc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d0abc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d0abc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d0abc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0abc-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d0abc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d0abc-118">Scenario description</span></span>
<span data-ttu-id="d0abc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d0abc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d0abc-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d0abc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d0abc-121">Přidání 10 000 ft plány z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d0abc-121">Adding 10,000ft Plans from hello gallery</span></span>
2. <span data-ttu-id="d0abc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d0abc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-10000ft-plans-from-hello-gallery"></a><span data-ttu-id="d0abc-123">Přidání 10 000 ft plány z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d0abc-123">Adding 10,000ft Plans from hello gallery</span></span>
<span data-ttu-id="d0abc-124">tooconfigure hello integrace plány 10 000 odolnosti proti selhání do Azure AD, je nutné tooadd 10 000 ft plány hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d0abc-124">tooconfigure hello integration of 10,000ft Plans into Azure AD, you need tooadd 10,000ft Plans from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d0abc-125">**tooadd 10 000 ft plány z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d0abc-125">**tooadd 10,000ft Plans from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0abc-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d0abc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d0abc-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d0abc-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d0abc-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d0abc-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d0abc-133">Hello vyhledávacího pole zadejte **10 000 ft plány**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-133">In hello search box, type **10,000ft Plans**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. <span data-ttu-id="d0abc-135">Na panelu výsledků hello vyberte **10 000 ft plány**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0abc-135">In hello results panel, select **10,000ft Plans**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d0abc-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d0abc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d0abc-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 10 000 ft plány podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="d0abc-138">In this section, you configure and test Azure AD single sign-on with 10,000ft Plans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d0abc-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v plánech úrovně 10 000 ft je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d0abc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 10,000ft Plans is tooa user in Azure AD.</span></span> <span data-ttu-id="d0abc-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v 10 000 ft plány musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d0abc-140">In other words, a link relationship between an Azure AD user and hello related user in 10,000ft Plans needs toobe established.</span></span>

<span data-ttu-id="d0abc-141">V plánech 10 000 ft přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d0abc-141">In 10,000ft Plans, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d0abc-142">tooconfigure a testu Azure AD jednotné přihlašování s 10 000 ft plány, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d0abc-142">tooconfigure and test Azure AD single sign-on with 10,000ft Plans, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d0abc-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d0abc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d0abc-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d0abc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d0abc-145">**[Vytváření 10 000 ft testovací plány uživatel](#creating-a-10000ft-plans-test-user)**  -toohave protějšek Britta Simon v 10 000 ft plánů, který je propojený reprezentace toohello Azure AD uživatele.</span><span class="sxs-lookup"><span data-stu-id="d0abc-145">**[Creating a 10,000ft Plans test user](#creating-a-10000ft-plans-test-user)** - toohave a counterpart of Britta Simon in 10,000ft Plans that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d0abc-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d0abc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d0abc-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d0abc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d0abc-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d0abc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d0abc-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci 10 000 ft plány.</span><span class="sxs-lookup"><span data-stu-id="d0abc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 10,000ft Plans application.</span></span>

<span data-ttu-id="d0abc-150">**tooconfigure Azure AD jednotné přihlašování s 10 000 ft plány, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d0abc-150">**tooconfigure Azure AD single sign-on with 10,000ft Plans, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0abc-151">V portálu Azure, na hello hello **10 000 ft plány** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-151">In hello Azure portal, on hello **10,000ft Plans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d0abc-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d0abc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. <span data-ttu-id="d0abc-155">Na hello **10 000 ft plány domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d0abc-155">On hello **10,000ft Plans Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    <span data-ttu-id="d0abc-157">a.</span><span class="sxs-lookup"><span data-stu-id="d0abc-157">a.</span></span> <span data-ttu-id="d0abc-158">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://app.10000ft.com`</span><span class="sxs-lookup"><span data-stu-id="d0abc-158">In hello **Sign-on URL** textbox, type hello URL: `https://app.10000ft.com`</span></span>

    <span data-ttu-id="d0abc-159">b.</span><span class="sxs-lookup"><span data-stu-id="d0abc-159">b.</span></span> <span data-ttu-id="d0abc-160">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://app.10000ft.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="d0abc-160">In hello **Identifier** textbox, type hello URL: `https://app.10000ft.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d0abc-161">Hello hodnotu **identifikátor** se liší, pokud máte vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="d0abc-161">hello value for **Identifier** is different if you have a custom domain.</span></span> <span data-ttu-id="d0abc-162">Obraťte se na [tým podpory plány 10 000 ft](https://www.10000ft.com/plans/support) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d0abc-162">Contact [10,000ft Plans support team](https://www.10000ft.com/plans/support) tooget this value.</span></span> 
 
4. <span data-ttu-id="d0abc-163">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d0abc-163">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. <span data-ttu-id="d0abc-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d0abc-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d0abc-167">Na hello **10 000 ft plány konfigurace** klikněte na tlačítko **10 000 ft plány nakonfigurovat** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d0abc-167">On hello **10,000ft Plans Configuration** section, click **Configure 10,000ft Plans** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d0abc-168">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d0abc-168">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. <span data-ttu-id="d0abc-170">tooconfigure jednotného přihlašování na **10 000 ft plány** straně, je nutné stáhnout hello toosend **Certificate(Raw), adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[ tým podpory plány 10 000 ft](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="d0abc-170">tooconfigure single sign-on on **10,000ft Plans** side, you need toosend hello downloaded **Certificate(Raw), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

> [!TIP]
> <span data-ttu-id="d0abc-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d0abc-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d0abc-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d0abc-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d0abc-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d0abc-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d0abc-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d0abc-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="d0abc-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d0abc-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d0abc-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d0abc-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0abc-178">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d0abc-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d0abc-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d0abc-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="d0abc-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d0abc-184">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d0abc-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d0abc-186">a.</span><span class="sxs-lookup"><span data-stu-id="d0abc-186">a.</span></span> <span data-ttu-id="d0abc-187">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d0abc-188">b.</span><span class="sxs-lookup"><span data-stu-id="d0abc-188">b.</span></span> <span data-ttu-id="d0abc-189">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d0abc-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d0abc-190">c.</span><span class="sxs-lookup"><span data-stu-id="d0abc-190">c.</span></span> <span data-ttu-id="d0abc-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d0abc-192">d.</span><span class="sxs-lookup"><span data-stu-id="d0abc-192">d.</span></span> <span data-ttu-id="d0abc-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-193">Click **Create**.</span></span>
 
### <a name="creating-a-10000ft-plans-test-user"></a><span data-ttu-id="d0abc-194">Vytváření 10 000 ft plány testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="d0abc-194">Creating a 10,000ft Plans test user</span></span>

<span data-ttu-id="d0abc-195">Hello cílem této části je toocreate názvem Britta Simon v plánech úrovně 10 000 ft uživatele.</span><span class="sxs-lookup"><span data-stu-id="d0abc-195">hello objective of this section is toocreate a user called Britta Simon in 10,000ft Plans.</span></span> <span data-ttu-id="d0abc-196">10 000 ft plány podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="d0abc-196">10,000ft Plans supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="d0abc-197">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="d0abc-197">There is no action item for you in this section.</span></span> <span data-ttu-id="d0abc-198">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess 10 000 ft plány.</span><span class="sxs-lookup"><span data-stu-id="d0abc-198">A new user is created during an attempt tooaccess 10,000ft Plans if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="d0abc-199">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory plány 10 000 ft](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="d0abc-199">If you need toocreate a user manually, you need toocontact hello [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d0abc-200">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d0abc-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d0abc-201">V této části povolíte tak, že udělíte přístup too10, plány 000ft Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d0abc-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too10,000ft Plans.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d0abc-203">**tooassign too10 Britta Simon 000ft plány, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d0abc-203">**tooassign Britta Simon too10,000ft Plans, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0abc-204">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d0abc-206">V seznamu aplikace hello vyberte **10 000 ft plány**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-206">In hello applications list, select **10,000ft Plans**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. <span data-ttu-id="d0abc-208">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d0abc-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d0abc-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d0abc-210">Click **Add** button.</span></span> <span data-ttu-id="d0abc-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d0abc-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d0abc-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d0abc-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d0abc-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d0abc-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d0abc-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d0abc-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d0abc-216">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d0abc-216">Testing single sign-on</span></span>

<span data-ttu-id="d0abc-217">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d0abc-217">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="d0abc-218">Po kliknutí na tlačítko hello 10 000 ft, které plány dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour 10 000 ft plány aplikace.</span><span class="sxs-lookup"><span data-stu-id="d0abc-218">When you click hello 10,000ft Plans tile in hello Access Panel, you should get automatically signed-on tooyour 10,000ft Plans application.</span></span>
 
## <a name="additional-resources"></a><span data-ttu-id="d0abc-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d0abc-219">Additional resources</span></span>

* [<span data-ttu-id="d0abc-220">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d0abc-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d0abc-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d0abc-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png

