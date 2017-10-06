---
title: 'Kurz: Azure Active Directory integrace s UserVoice | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a služby UserVoice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 9eade8435ae6c6a3821bbbec9ab7c27ed7ad91ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a><span data-ttu-id="9cafd-103">Kurz: Azure Active Directory integrace s UserVoice</span><span class="sxs-lookup"><span data-stu-id="9cafd-103">Tutorial: Azure Active Directory integration with UserVoice</span></span>

<span data-ttu-id="9cafd-104">V tomto kurzu zjistíte, jak toointegrate UserVoice s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9cafd-104">In this tutorial, you learn how toointegrate UserVoice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9cafd-105">Integrace služby UserVoice s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9cafd-105">Integrating UserVoice with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9cafd-106">Můžete ovládat ve službě Azure AD, který má přístup tooUserVoice.</span><span class="sxs-lookup"><span data-stu-id="9cafd-106">You can control in Azure AD who has access tooUserVoice.</span></span>
- <span data-ttu-id="9cafd-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooUserVoice (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9cafd-107">You can enable your users tooautomatically get signed-on tooUserVoice (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9cafd-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9cafd-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="9cafd-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9cafd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9cafd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9cafd-110">Prerequisites</span></span>

<span data-ttu-id="9cafd-111">Integrace služby Azure AD s UserVoice tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9cafd-111">tooconfigure Azure AD integration with UserVoice, you need hello following items:</span></span>

- <span data-ttu-id="9cafd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cafd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9cafd-113">Webu UserVoice jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9cafd-113">A UserVoice single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9cafd-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9cafd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9cafd-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9cafd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9cafd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9cafd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9cafd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9cafd-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9cafd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9cafd-118">Scenario description</span></span>
<span data-ttu-id="9cafd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9cafd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9cafd-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9cafd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9cafd-121">Přidání UserVoice z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9cafd-121">Adding UserVoice from hello gallery</span></span>
2. <span data-ttu-id="9cafd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9cafd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-uservoice-from-hello-gallery"></a><span data-ttu-id="9cafd-123">Přidání UserVoice z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9cafd-123">Adding UserVoice from hello gallery</span></span>
<span data-ttu-id="9cafd-124">tooconfigure hello integrace UserVoice do Azure AD, je nutné tooadd UserVoice hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9cafd-124">tooconfigure hello integration of UserVoice into Azure AD, you need tooadd UserVoice from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9cafd-125">**tooadd UserVoice z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9cafd-125">**tooadd UserVoice from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cafd-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9cafd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="9cafd-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9cafd-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="9cafd-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9cafd-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="9cafd-133">Hello vyhledávacího pole zadejte **UserVoice**, vyberte **UserVoice** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cafd-133">In hello search box, type **UserVoice**, select **UserVoice** from result panel then click **Add** button tooadd hello application.</span></span>

    ![V seznamu výsledků hello UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9cafd-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9cafd-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="9cafd-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UserVoice podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9cafd-136">In this section, you configure and test Azure AD single sign-on with UserVoice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9cafd-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v UserVoice je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9cafd-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UserVoice is tooa user in Azure AD.</span></span> <span data-ttu-id="9cafd-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v UserVoice musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9cafd-138">In other words, a link relationship between an Azure AD user and hello related user in UserVoice needs toobe established.</span></span>

<span data-ttu-id="9cafd-139">V UserVoice, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="9cafd-139">In UserVoice, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9cafd-140">tooconfigure a testu Azure AD jednotné přihlašování s UserVoice, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9cafd-140">tooconfigure and test Azure AD single sign-on with UserVoice, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9cafd-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9cafd-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9cafd-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9cafd-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9cafd-143">**[Vytvoření zkušebního uživatele UserVoice](#create-a-uservoice-test-user)**  -toohave protějšek Britta Simon v UserVoice, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="9cafd-143">**[Create a UserVoice test user](#create-a-uservoice-test-user)** - toohave a counterpart of Britta Simon in UserVoice that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9cafd-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9cafd-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9cafd-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9cafd-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9cafd-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9cafd-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9cafd-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci UserVoice.</span><span class="sxs-lookup"><span data-stu-id="9cafd-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UserVoice application.</span></span>

<span data-ttu-id="9cafd-148">**tooconfigure Azure AD jednotné přihlašování s UserVoice, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9cafd-148">**tooconfigure Azure AD single sign-on with UserVoice, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cafd-149">V portálu Azure, na hello hello **UserVoice** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-149">In hello Azure portal, on hello **UserVoice** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="9cafd-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9cafd-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. <span data-ttu-id="9cafd-153">Na hello **UserVoice domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9cafd-153">On hello **UserVoice Domain and URLs** section, perform hello following steps:</span></span>

    ![Doména služby UserVoice a adresy URL jednotné přihlašování informace](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    <span data-ttu-id="9cafd-155">a.</span><span class="sxs-lookup"><span data-stu-id="9cafd-155">a.</span></span> <span data-ttu-id="9cafd-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="9cafd-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    <span data-ttu-id="9cafd-157">b.</span><span class="sxs-lookup"><span data-stu-id="9cafd-157">b.</span></span> <span data-ttu-id="9cafd-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="9cafd-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9cafd-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9cafd-159">These values are not real.</span></span> <span data-ttu-id="9cafd-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9cafd-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9cafd-161">Obraťte se na [tým podpory UserVoice klienta](https://www.uservoice.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9cafd-161">Contact [UserVoice Client support team](https://www.uservoice.com/) tooget these values.</span></span>

4. <span data-ttu-id="9cafd-162">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9cafd-162">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. <span data-ttu-id="9cafd-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9cafd-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9cafd-166">Na hello **UserVoice konfigurace** klikněte na tlačítko **konfigurace UserVoice** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9cafd-166">On hello **UserVoice Configuration** section, click **Configure UserVoice** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9cafd-167">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9cafd-167">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace služby UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. <span data-ttu-id="9cafd-169">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour UserVoice.</span><span class="sxs-lookup"><span data-stu-id="9cafd-169">In a different web browser window, log in tooyour UserVoice company site as an administrator.</span></span>

8. <span data-ttu-id="9cafd-170">V panelu nástrojů hello hello nahoře, klikněte na **nastavení**a potom vyberte **webový portál** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="9cafd-170">In hello toolbar on hello top, click **Settings**, and then select **Web portal** from hello menu.</span></span>
   
    <span data-ttu-id="9cafd-171">![V oddílu nastavení na straně aplikace](./media/active-directory-saas-uservoice-tutorial/ic777519.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="9cafd-171">![Settings Section On App Side](./media/active-directory-saas-uservoice-tutorial/ic777519.png "Settings")</span></span>

9. <span data-ttu-id="9cafd-172">Na hello **webový portál** na kartě hello **ověření uživatele** klikněte na tlačítko **upravit** tooopen hello **upravit ověření uživatele** dialogové okno stránka.</span><span class="sxs-lookup"><span data-stu-id="9cafd-172">On hello **Web portal** tab, in hello **User authentication** section, click **Edit** tooopen hello **Edit User Authentication** dialog page.</span></span>
   
    <span data-ttu-id="9cafd-173">![Webový portál karta](./media/active-directory-saas-uservoice-tutorial/ic777520.png "webový portál")</span><span class="sxs-lookup"><span data-stu-id="9cafd-173">![Web portal Tab](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web portal")</span></span>

10. <span data-ttu-id="9cafd-174">Na hello **upravit ověření uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9cafd-174">On hello **Edit User Authentication** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="9cafd-175">![Upravit ověření uživatele](./media/active-directory-saas-uservoice-tutorial/ic777521.png "upravit ověření uživatele")</span><span class="sxs-lookup"><span data-stu-id="9cafd-175">![Edit user authentication](./media/active-directory-saas-uservoice-tutorial/ic777521.png "Edit user authentication")</span></span>
   
    <span data-ttu-id="9cafd-176">a.</span><span class="sxs-lookup"><span data-stu-id="9cafd-176">a.</span></span> <span data-ttu-id="9cafd-177">Klikněte na tlačítko **jednotné přihlašování (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-177">Click **Single Sign-On (SSO)**.</span></span>
 
    <span data-ttu-id="9cafd-178">b.</span><span class="sxs-lookup"><span data-stu-id="9cafd-178">b.</span></span> <span data-ttu-id="9cafd-179">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **jednotné přihlašování vzdáleného přihlášení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9cafd-179">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **SSO Remote Sign-In** textbox.</span></span>

    <span data-ttu-id="9cafd-180">c.</span><span class="sxs-lookup"><span data-stu-id="9cafd-180">c.</span></span> <span data-ttu-id="9cafd-181">Vložení hello **Sign-Out URL** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **vzdáleného Sign-Out jednotného přihlašování k textovému poli**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-181">Paste hello **Sign-Out URL** value, which you have copied from hello Azure portal into hello **SSO Remote Sign-Out textbox**.</span></span>
 
    <span data-ttu-id="9cafd-182">d.</span><span class="sxs-lookup"><span data-stu-id="9cafd-182">d.</span></span> <span data-ttu-id="9cafd-183">Vložení hello **kryptografický otisk** hodnotu, kterou jste zkopírovali z portálu Azure do **aktuální otisků prstů certifikátů SHA1** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9cafd-183">Paste hello **Thumbprint** value , which you have copied from Azure portal  into the **Current certificate SHA1 fingerprint** textbox.</span></span>
    
    <span data-ttu-id="9cafd-184">e.</span><span class="sxs-lookup"><span data-stu-id="9cafd-184">e.</span></span> <span data-ttu-id="9cafd-185">Klikněte na tlačítko **uložit nastavení ověřování**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-185">Click **Save authentication settings**.</span></span>

> [!TIP]
> <span data-ttu-id="9cafd-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="9cafd-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9cafd-187">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="9cafd-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9cafd-188">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9cafd-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9cafd-189">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cafd-189">Create an Azure AD test user</span></span>

<span data-ttu-id="9cafd-190">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="9cafd-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="9cafd-192">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9cafd-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cafd-193">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9cafd-193">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9cafd-195">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-195">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9cafd-197">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9cafd-197">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9cafd-199">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9cafd-199">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9cafd-201">a.</span><span class="sxs-lookup"><span data-stu-id="9cafd-201">a.</span></span> <span data-ttu-id="9cafd-202">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-202">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9cafd-203">b.</span><span class="sxs-lookup"><span data-stu-id="9cafd-203">b.</span></span> <span data-ttu-id="9cafd-204">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9cafd-204">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="9cafd-205">c.</span><span class="sxs-lookup"><span data-stu-id="9cafd-205">c.</span></span> <span data-ttu-id="9cafd-206">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="9cafd-206">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="9cafd-207">d.</span><span class="sxs-lookup"><span data-stu-id="9cafd-207">d.</span></span> <span data-ttu-id="9cafd-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-208">Click **Create**.</span></span>
 
### <a name="create-a-uservoice-test-user"></a><span data-ttu-id="9cafd-209">Vytvoření zkušebního uživatele UserVoice</span><span class="sxs-lookup"><span data-stu-id="9cafd-209">Create a UserVoice test user</span></span>

<span data-ttu-id="9cafd-210">Uživatelé toolog tooenable Azure AD v tooUserVoice, se musí být zřízená do UserVoice.</span><span class="sxs-lookup"><span data-stu-id="9cafd-210">tooenable Azure AD users toolog in tooUserVoice, they must be provisioned into UserVoice.</span></span> <span data-ttu-id="9cafd-211">V případě hello služby UserVoice zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="9cafd-211">In hello case of UserVoice, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="9cafd-212">tooprovision uživatelský účet, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9cafd-212">tooprovision a user account, perform hello following steps:</span></span>
1. <span data-ttu-id="9cafd-213">Přihlaste se tooyour **UserVoice** klienta.</span><span class="sxs-lookup"><span data-stu-id="9cafd-213">Log in tooyour **UserVoice** tenant.</span></span>

2. <span data-ttu-id="9cafd-214">Přejděte příliš**nastavení**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-214">Go too**Settings**.</span></span>
   
    <span data-ttu-id="9cafd-215">![Nastavení](./media/active-directory-saas-uservoice-tutorial/ic777811.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="9cafd-215">![Settings](./media/active-directory-saas-uservoice-tutorial/ic777811.png "Settings")</span></span>

3. <span data-ttu-id="9cafd-216">Klikněte na tlačítko **Obecné**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-216">Click **General**.</span></span>

4. <span data-ttu-id="9cafd-217">Klikněte na tlačítko **agenty a oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-217">Click **Agents and permissions**.</span></span>
   
    <span data-ttu-id="9cafd-218">![Agenti a oprávnění](./media/active-directory-saas-uservoice-tutorial/ic777812.png "agenty a oprávnění")</span><span class="sxs-lookup"><span data-stu-id="9cafd-218">![Agents and permissions](./media/active-directory-saas-uservoice-tutorial/ic777812.png "Agents and permissions")</span></span>

5. <span data-ttu-id="9cafd-219">Klikněte na tlačítko **přidat admins**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-219">Click **Add admins**.</span></span>
   
    <span data-ttu-id="9cafd-220">![Přidat admins](./media/active-directory-saas-uservoice-tutorial/ic777813.png "přidat admins")</span><span class="sxs-lookup"><span data-stu-id="9cafd-220">![Add admins](./media/active-directory-saas-uservoice-tutorial/ic777813.png "Add admins")</span></span>

6. <span data-ttu-id="9cafd-221">Na hello **pozvat správci** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9cafd-221">On hello **Invite admins** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="9cafd-222">![Pozvat správci](./media/active-directory-saas-uservoice-tutorial/ic777814.png "pozvat správci")</span><span class="sxs-lookup"><span data-stu-id="9cafd-222">![Invite admins](./media/active-directory-saas-uservoice-tutorial/ic777814.png "Invite admins")</span></span>
   
    <span data-ttu-id="9cafd-223">a.</span><span class="sxs-lookup"><span data-stu-id="9cafd-223">a.</span></span> <span data-ttu-id="9cafd-224">V textovém poli hello e-mailů, zadejte hello e-mailovou adresu účtu hello tooprovision a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-224">In hello Emails textbox, type hello email address of hello account you want tooprovision, and then click **Add**.</span></span>
   
    <span data-ttu-id="9cafd-225">b.</span><span class="sxs-lookup"><span data-stu-id="9cafd-225">b.</span></span> <span data-ttu-id="9cafd-226">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-226">Click **Invite**.</span></span>

> [!NOTE]
> <span data-ttu-id="9cafd-227">Můžete použít všechny ostatní UserVoice uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované UserVoice tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="9cafd-227">You can use any other UserVoice user account creation tools or APIs provided by UserVoice tooprovision AAD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9cafd-228">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9cafd-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9cafd-229">V této části povolíte tak, že udělíte přístup tooUserVoice toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9cafd-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUserVoice.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="9cafd-231">**tooassign Britta Simon tooUserVoice, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9cafd-231">**tooassign Britta Simon tooUserVoice, perform hello following steps:**</span></span>

1. <span data-ttu-id="9cafd-232">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9cafd-234">V seznamu aplikace hello vyberte **UserVoice**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-234">In hello applications list, select **UserVoice**.</span></span>

    ![v seznamu aplikace hello Hello UserVoice odkaz](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. <span data-ttu-id="9cafd-236">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9cafd-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="9cafd-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9cafd-238">Click **Add** button.</span></span> <span data-ttu-id="9cafd-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9cafd-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="9cafd-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9cafd-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9cafd-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9cafd-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9cafd-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9cafd-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9cafd-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9cafd-244">Test single sign-on</span></span>

<span data-ttu-id="9cafd-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9cafd-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9cafd-246">Když kliknete na dlaždici UserVoice hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour UserVoice aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cafd-246">When you click hello UserVoice tile in hello Access Panel, you should get automatically signed-on tooyour UserVoice application.</span></span>
<span data-ttu-id="9cafd-247">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9cafd-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9cafd-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9cafd-248">Additional resources</span></span>

* [<span data-ttu-id="9cafd-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9cafd-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9cafd-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9cafd-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

