---
title: 'Kurz: Azure Active Directory integrace s Dropbox pro firmy | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Dropbox pro firmy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3f33a43ca8fbd60486d7a400ae8246af768376ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a><span data-ttu-id="82722-103">Kurz: Azure Active Directory integrace s Dropbox pro firmy</span><span class="sxs-lookup"><span data-stu-id="82722-103">Tutorial: Azure Active Directory integration with Dropbox for Business</span></span>

<span data-ttu-id="82722-104">V tomto kurzu zjistíte, jak toointegrate Dropbox pro firmy s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="82722-104">In this tutorial, you learn how toointegrate Dropbox for Business with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="82722-105">Integrace Dropbox pro firmy s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="82722-105">Integrating Dropbox for Business with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="82722-106">Můžete řídit ve službě Azure AD, který má přístup tooDropbox pro firmy</span><span class="sxs-lookup"><span data-stu-id="82722-106">You can control in Azure AD who has access tooDropbox for Business</span></span>
- <span data-ttu-id="82722-107">Vaši uživatelé tooautomatically get přihlášeného tooDropbox můžete povolit pro firmy (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="82722-107">You can enable your users tooautomatically get signed-on tooDropbox for Business (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="82722-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="82722-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="82722-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="82722-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82722-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="82722-110">Prerequisites</span></span>

<span data-ttu-id="82722-111">tooconfigure integrace Azure AD s Dropbox pro firmy, musíte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="82722-111">tooconfigure Azure AD integration with Dropbox for Business, you need hello following items:</span></span>

- <span data-ttu-id="82722-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="82722-112">An Azure AD subscription</span></span>
- <span data-ttu-id="82722-113">Dropbox pro obchodní jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="82722-113">A Dropbox for Business single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="82722-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="82722-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="82722-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="82722-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="82722-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="82722-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="82722-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82722-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="82722-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="82722-118">Scenario description</span></span>
<span data-ttu-id="82722-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="82722-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="82722-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="82722-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="82722-121">Přidání Dropbox pro firmy z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="82722-121">Adding Dropbox for Business from hello gallery</span></span>
2. <span data-ttu-id="82722-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="82722-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dropbox-for-business-from-hello-gallery"></a><span data-ttu-id="82722-123">Přidání Dropbox pro firmy z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="82722-123">Adding Dropbox for Business from hello gallery</span></span>
<span data-ttu-id="82722-124">integrace hello tooconfigure Dropbox pro firmy do služby Azure AD, musíte pro firmy hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd Dropbox.</span><span class="sxs-lookup"><span data-stu-id="82722-124">tooconfigure hello integration of Dropbox for Business into Azure AD, you need tooadd Dropbox for Business from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="82722-125">**tooadd Dropbox pro firmy z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82722-125">**tooadd Dropbox for Business from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="82722-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="82722-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="82722-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="82722-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="82722-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="82722-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="82722-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82722-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="82722-133">Hello vyhledávacího pole zadejte **Dropbox pro firmy**.</span><span class="sxs-lookup"><span data-stu-id="82722-133">In hello search box, type **Dropbox for Business**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. <span data-ttu-id="82722-135">Na panelu výsledků hello vyberte **Dropbox pro firmy**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="82722-135">In hello results panel, select **Dropbox for Business**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="82722-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="82722-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="82722-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox pro firmy na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="82722-138">In this section, you configure and test Azure AD single sign-on with Dropbox for Business based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="82722-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Dropbox pro firmy je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82722-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Dropbox for Business is tooa user in Azure AD.</span></span> <span data-ttu-id="82722-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Dropbox pro firmy musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="82722-140">In other words, a link relationship between an Azure AD user and hello related user in Dropbox for Business needs toobe established.</span></span>

<span data-ttu-id="82722-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="82722-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Dropbox for Business.</span></span>

<span data-ttu-id="82722-142">tooconfigure a testu Azure AD jednotné přihlašování s Dropbox pro firmy, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="82722-142">tooconfigure and test Azure AD single sign-on with Dropbox for Business, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="82722-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="82722-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="82722-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82722-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="82722-145">**[Vytváření Dropbox pro obchodní testovacího uživatele](#creating-a-dropbox-for-business-test-user)**  -toohave protějšek Britta Simon v Dropbox pro firmy, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="82722-145">**[Creating a Dropbox for Business test user](#creating-a-dropbox-for-business-test-user)** - toohave a counterpart of Britta Simon in Dropbox for Business that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="82722-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82722-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="82722-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="82722-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="82722-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="82722-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="82722-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování do vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="82722-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Dropbox for Business application.</span></span>

<span data-ttu-id="82722-150">**tooconfigure Azure AD jednotné přihlašování s Dropbox pro firmy, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82722-150">**tooconfigure Azure AD single sign-on with Dropbox for Business, perform hello following steps:**</span></span>

1. <span data-ttu-id="82722-151">V portálu Azure, na hello hello **Dropbox pro firmy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="82722-151">In hello Azure portal, on hello **Dropbox for Business** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="82722-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82722-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. <span data-ttu-id="82722-155">Na hello **Dropbox obchodní domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="82722-155">On hello **Dropbox for Business Domain and URLs** section, perform hello following steps:</span></span>

    <span data-ttu-id="82722-156">a.</span><span class="sxs-lookup"><span data-stu-id="82722-156">a.</span></span> <span data-ttu-id="82722-157">Přihlášení tooyour Dropbox pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="82722-157">Sign on tooyour Dropbox for business tenant.</span></span> 
   
    <span data-ttu-id="82722-158">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="82722-158">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="82722-159">b.</span><span class="sxs-lookup"><span data-stu-id="82722-159">b.</span></span> <span data-ttu-id="82722-160">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="82722-160">In hello navigation pane on hello left side, click **Admin Console**.</span></span> 
   
    <span data-ttu-id="82722-161">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="82722-161">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="82722-162">c.</span><span class="sxs-lookup"><span data-stu-id="82722-162">c.</span></span> <span data-ttu-id="82722-163">Na hello **konzoly pro správu**, klikněte na tlačítko **ověřování** v levém navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="82722-163">On hello **Admin Console**, click **Authentication** in hello left navigation pane.</span></span> 
   
    <span data-ttu-id="82722-164">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="82722-164">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="82722-165">d.</span><span class="sxs-lookup"><span data-stu-id="82722-165">d.</span></span> <span data-ttu-id="82722-166">V hello **jednotného přihlašování** vyberte **povolit jednotné přihlašování**a potom klikněte na **Další** tooexpand v této části.</span><span class="sxs-lookup"><span data-stu-id="82722-166">In hello **Single sign-on** section, select **Enable single sign-on**, and then click **More** tooexpand this section.</span></span>  
   
    <span data-ttu-id="82722-167">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="82722-167">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="82722-168">e.</span><span class="sxs-lookup"><span data-stu-id="82722-168">e.</span></span> <span data-ttu-id="82722-169">Zkopírujte adresu URL hello vedle příliš**uživatelé se mohou přihlásit zadáním e-mailové adresy nebo můžete přejít přímo na**.</span><span class="sxs-lookup"><span data-stu-id="82722-169">Copy hello URL next too**Users can sign in by entering their email address or they can go directly to**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    <span data-ttu-id="82722-171">f.</span><span class="sxs-lookup"><span data-stu-id="82722-171">f.</span></span> <span data-ttu-id="82722-172">Na portálu Azure, v hello hello **přihlašovací adresa URL** textovému poli, hello vložte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="82722-172">On hello Azure portal, in hello **Sign-on URL** textbox, paste hello URL.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     <span data-ttu-id="82722-174">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.dropbox.com/sso/<id>`</span><span class="sxs-lookup"><span data-stu-id="82722-174">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.dropbox.com/sso/<id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="82722-175">Tato hodnota není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="82722-175">This value is not real value.</span></span> <span data-ttu-id="82722-176">Aktualizujte hodnotu hello s hello skutečná adresa URL přihlašování můžete získat z jejich části jednom přihlášení.</span><span class="sxs-lookup"><span data-stu-id="82722-176">Update hello value with hello actual Sign-on URL you get from their Single sign-on section.</span></span> <span data-ttu-id="82722-177">Obraťte se na [Dropbox pro klienta obchodní tým podpory](https://www.dropbox.com/business/contact) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="82722-177">Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact) tooget this value.</span></span> 
 
4. <span data-ttu-id="82722-178">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="82722-178">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. <span data-ttu-id="82722-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82722-180">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="82722-182">Na hello **Dropbox pro konfiguraci obchodní** klikněte na tlačítko **Dropbox nakonfigurovat pro firmy** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="82722-182">On hello **Dropbox for Business Configuration** section, click **Configure Dropbox for Business** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="82722-183">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="82722-183">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. <span data-ttu-id="82722-185">tooconfigure jednotného přihlašování na **Dropbox pro firmy** straně, přejděte na vaší schránky pro obchodní klienta v hello **jednotného přihlašování** části hello **ověřování** stránky, Proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="82722-185">tooconfigure single sign-on on **Dropbox for Business** side, Go on your Dropbox for Business tenant, in hello **Single sign-on** section of hello **Authentication** page, perform hello following steps:</span></span> 
   
    <span data-ttu-id="82722-186">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="82722-186">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="82722-187">a.</span><span class="sxs-lookup"><span data-stu-id="82722-187">a.</span></span> <span data-ttu-id="82722-188">Klikněte na tlačítko **požadované**.</span><span class="sxs-lookup"><span data-stu-id="82722-188">Click **Required**.</span></span>
   
    <span data-ttu-id="82722-189">b.</span><span class="sxs-lookup"><span data-stu-id="82722-189">b.</span></span> <span data-ttu-id="82722-190">V portálu Azure, na hello hello **konfigurovat přihlášení** okno, kopie hello **SAML jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="82722-190">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **Sign-in URL** textbox.</span></span>

    <span data-ttu-id="82722-191">c.</span><span class="sxs-lookup"><span data-stu-id="82722-191">c.</span></span> <span data-ttu-id="82722-192">Klikněte na tlačítko **vyberte certifikát**a potom vyhledejte tooyour **Base64 kódovaného certifikátu souboru**.</span><span class="sxs-lookup"><span data-stu-id="82722-192">Click **Choose certificate**, and then browse tooyour **Base64 encoded certificate file**.</span></span>

    <span data-ttu-id="82722-193">d.</span><span class="sxs-lookup"><span data-stu-id="82722-193">d.</span></span> <span data-ttu-id="82722-194">Klikněte na tlačítko **uložit změny** toocomplete hello konfiguraci na vaší schránky pro obchodní klienta.</span><span class="sxs-lookup"><span data-stu-id="82722-194">Click **Save changes** toocomplete hello configuration on your DropBox for Business tenant.</span></span>

> [!TIP]
> <span data-ttu-id="82722-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="82722-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="82722-196">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="82722-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="82722-197">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="82722-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="82722-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="82722-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="82722-199">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="82722-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="82722-201">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82722-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="82722-202">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="82722-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="82722-204">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="82722-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="82722-206">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82722-206">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="82722-208">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="82722-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="82722-210">a.</span><span class="sxs-lookup"><span data-stu-id="82722-210">a.</span></span> <span data-ttu-id="82722-211">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="82722-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="82722-212">b.</span><span class="sxs-lookup"><span data-stu-id="82722-212">b.</span></span> <span data-ttu-id="82722-213">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="82722-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="82722-214">c.</span><span class="sxs-lookup"><span data-stu-id="82722-214">c.</span></span> <span data-ttu-id="82722-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="82722-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="82722-216">d.</span><span class="sxs-lookup"><span data-stu-id="82722-216">d.</span></span> <span data-ttu-id="82722-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="82722-217">Click **Create**.</span></span>
 
### <a name="creating-a-dropbox-for-business-test-user"></a><span data-ttu-id="82722-218">Vytváření Dropbox pro obchodní testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="82722-218">Creating a Dropbox for Business test user</span></span>

<span data-ttu-id="82722-219">V této části se uživatel volá Britta Simon vytvoří v Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="82722-219">In this section, a user called Britta Simon is created in Dropbox for Business.</span></span> <span data-ttu-id="82722-220">Dropbox pro firmy podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="82722-220">Dropbox for Business supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="82722-221">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="82722-221">There is no action item for you in this section.</span></span> <span data-ttu-id="82722-222">Pokud uživatel ještě neexistuje v Dropbox pro firmy, je vytvořen nový pokusíte-li tooaccess Dropbox pro firmy.</span><span class="sxs-lookup"><span data-stu-id="82722-222">If a user doesn't already exist in Dropbox for Business, a new one is created when you attempt tooaccess Dropbox for Business.</span></span>

>[!Note]
><span data-ttu-id="82722-223">Pokud potřebujete toocreate uživatel ručně, obraťte se na [Dropbox pro tým podpory obchodní klienta](https://www.dropbox.com/business/contact)</span><span class="sxs-lookup"><span data-stu-id="82722-223">If you need toocreate a user manually, Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact)</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="82722-224">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="82722-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="82722-225">V této části povolíte tak, že udělíte přístup tooDropbox pro firmy Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82722-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDropbox for Business.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="82722-227">**tooassign tooDropbox Britta Simon pro firmy, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="82722-227">**tooassign Britta Simon tooDropbox for Business, perform hello following steps:**</span></span>

1. <span data-ttu-id="82722-228">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="82722-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="82722-230">V seznamu aplikace hello vyberte **Dropbox pro firmy**.</span><span class="sxs-lookup"><span data-stu-id="82722-230">In hello applications list, select **Dropbox for Business**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. <span data-ttu-id="82722-232">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="82722-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="82722-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82722-234">Click **Add** button.</span></span> <span data-ttu-id="82722-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82722-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="82722-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="82722-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="82722-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82722-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="82722-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82722-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="82722-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="82722-240">Testing single sign-on</span></span>

<span data-ttu-id="82722-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="82722-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="82722-242">Po kliknutí na tlačítko Dropbox hello pro firmy dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku vaší schránky pro obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="82722-242">When you click hello Dropbox for Business tile in hello Access Panel, you should get login page of your Dropbox for Business application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82722-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="82722-243">Additional resources</span></span>

* [<span data-ttu-id="82722-244">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="82722-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="82722-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="82722-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="82722-246">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="82722-246">Configure User Provisioning</span></span>](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

