---
title: 'Kurz: Azure Active Directory integrace s ThirdLight | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ThirdLight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: a510e514f6a8c4e89220b9a6f6db29668b451b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a><span data-ttu-id="70099-103">Kurz: Azure Active Directory integrace s ThirdLight</span><span class="sxs-lookup"><span data-stu-id="70099-103">Tutorial: Azure Active Directory integration with ThirdLight</span></span>

<span data-ttu-id="70099-104">V tomto kurzu zjistíte, jak toointegrate ThirdLight s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="70099-104">In this tutorial, you learn how toointegrate ThirdLight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="70099-105">Integrace ThirdLight s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="70099-105">Integrating ThirdLight with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="70099-106">Můžete řídit ve službě Azure AD, který má přístup tooThirdLight</span><span class="sxs-lookup"><span data-stu-id="70099-106">You can control in Azure AD who has access tooThirdLight</span></span>
- <span data-ttu-id="70099-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooThirdLight (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="70099-107">You can enable your users tooautomatically get signed-on tooThirdLight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="70099-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="70099-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="70099-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="70099-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70099-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70099-110">Prerequisites</span></span>

<span data-ttu-id="70099-111">Integrace služby Azure AD s ThirdLight tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="70099-111">tooconfigure Azure AD integration with ThirdLight, you need hello following items:</span></span>

- <span data-ttu-id="70099-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="70099-112">An Azure AD subscription</span></span>
- <span data-ttu-id="70099-113">ThirdLight jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="70099-113">A ThirdLight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="70099-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="70099-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="70099-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="70099-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="70099-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="70099-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="70099-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="70099-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="70099-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="70099-118">Scenario description</span></span>
<span data-ttu-id="70099-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="70099-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="70099-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="70099-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="70099-121">Přidání ThirdLight z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="70099-121">Adding ThirdLight from hello gallery</span></span>
2. <span data-ttu-id="70099-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="70099-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thirdlight-from-hello-gallery"></a><span data-ttu-id="70099-123">Přidání ThirdLight z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="70099-123">Adding ThirdLight from hello gallery</span></span>
<span data-ttu-id="70099-124">tooconfigure hello integrace ThirdLight do Azure AD, je nutné tooadd ThirdLight hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="70099-124">tooconfigure hello integration of ThirdLight into Azure AD, you need tooadd ThirdLight from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="70099-125">**tooadd ThirdLight z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70099-125">**tooadd ThirdLight from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="70099-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="70099-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="70099-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="70099-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="70099-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70099-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="70099-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70099-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="70099-133">Hello vyhledávacího pole zadejte **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="70099-133">In hello search box, type **ThirdLight**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_search.png)

5. <span data-ttu-id="70099-135">Na panelu výsledků hello vyberte **ThirdLight**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="70099-135">In hello results panel, select **ThirdLight**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="70099-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="70099-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="70099-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ThirdLight podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="70099-138">In this section, you configure and test Azure AD single sign-on with ThirdLight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="70099-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ThirdLight je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70099-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ThirdLight is tooa user in Azure AD.</span></span> <span data-ttu-id="70099-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ThirdLight musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="70099-140">In other words, a link relationship between an Azure AD user and hello related user in ThirdLight needs toobe established.</span></span>

<span data-ttu-id="70099-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="70099-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ThirdLight.</span></span>

<span data-ttu-id="70099-142">tooconfigure a testu Azure AD jednotné přihlašování s ThirdLight, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="70099-142">tooconfigure and test Azure AD single sign-on with ThirdLight, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="70099-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="70099-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="70099-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70099-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="70099-145">**[Vytvoření zkušebního uživatele ThirdLight](#creating-a-thirdlight-test-user)**  -toohave protějšek Britta Simon v ThirdLight, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="70099-145">**[Creating a ThirdLight test user](#creating-a-thirdlight-test-user)** - toohave a counterpart of Britta Simon in ThirdLight that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="70099-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70099-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="70099-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="70099-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="70099-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="70099-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="70099-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="70099-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ThirdLight application.</span></span>

<span data-ttu-id="70099-150">**tooconfigure Azure AD jednotné přihlašování s ThirdLight, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70099-150">**tooconfigure Azure AD single sign-on with ThirdLight, perform hello following steps:**</span></span>

1. <span data-ttu-id="70099-151">V portálu Azure, na hello hello **ThirdLight** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="70099-151">In hello Azure portal, on hello **ThirdLight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="70099-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70099-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_samlbase.png)

3. <span data-ttu-id="70099-155">Na hello **ThirdLight domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="70099-155">On hello **ThirdLight Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_url.png)

    <span data-ttu-id="70099-157">a.</span><span class="sxs-lookup"><span data-stu-id="70099-157">a.</span></span> <span data-ttu-id="70099-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.thirdlight.com/`</span><span class="sxs-lookup"><span data-stu-id="70099-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.thirdlight.com/`</span></span>

    <span data-ttu-id="70099-159">b.</span><span class="sxs-lookup"><span data-stu-id="70099-159">b.</span></span> <span data-ttu-id="70099-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.thirdlight.com/saml/sp`</span><span class="sxs-lookup"><span data-stu-id="70099-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.thirdlight.com/saml/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="70099-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="70099-161">These values are not real.</span></span> <span data-ttu-id="70099-162">Aktualizovat tyto hodnoty s hello skutečná adresa URL přihlašování a Identiifer.</span><span class="sxs-lookup"><span data-stu-id="70099-162">Update these values with hello actual Sign-On URL and Identiifer.</span></span> <span data-ttu-id="70099-163">Obraťte se na [tým podpory ThirdLight klienta](https://www.thirdlight.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="70099-163">Contact [ThirdLight Client support team](https://www.thirdlight.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="70099-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="70099-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_certificate.png) 

5. <span data-ttu-id="70099-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70099-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="70099-168">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti ThirdLight tooyour.</span><span class="sxs-lookup"><span data-stu-id="70099-168">In a different web browser window, log in tooyour ThirdLight company site as an administrator.</span></span>

7. <span data-ttu-id="70099-169">Přejděte příliš**konfigurace \> systému správy**a potom klikněte na **typu SAML2**.</span><span class="sxs-lookup"><span data-stu-id="70099-169">Go too**Configuration \> System Administration**, and then click **SAML2**.</span></span>
   
    <span data-ttu-id="70099-170">![Správa systému](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "správu systému")</span><span class="sxs-lookup"><span data-stu-id="70099-170">![System Administration](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "System Administration")</span></span>

8. <span data-ttu-id="70099-171">V konfiguračním oddílu hello typu SAML2 proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="70099-171">In hello SAML2 configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="70099-172">![SAML Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="70099-172">![SAML Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML Single Sign-On")</span></span>   

     <span data-ttu-id="70099-173">a.</span><span class="sxs-lookup"><span data-stu-id="70099-173">a.</span></span> <span data-ttu-id="70099-174">Vyberte **povolit typu SAML2 Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="70099-174">Select **Enable SAML2 Single Sign-On**.</span></span>
 
     <span data-ttu-id="70099-175">b.</span><span class="sxs-lookup"><span data-stu-id="70099-175">b.</span></span> <span data-ttu-id="70099-176">Jako **zdroj pro IdP Metadata**, vyberte **zatížení IdP metadat ze souboru XML**.</span><span class="sxs-lookup"><span data-stu-id="70099-176">As **Source for IdP Metadata**, select **Load IdP Metadata from XML**.</span></span>
 
     <span data-ttu-id="70099-177">c.</span><span class="sxs-lookup"><span data-stu-id="70099-177">c.</span></span> <span data-ttu-id="70099-178">Otevřete soubor metadat hello stáhli, kopírovat obsah hello a pak ji vložit do hello **soubor XML s metadaty IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="70099-178">Open hello downloaded metadata file, copy hello content, and then paste it into hello **IdP Metadata XML** textbox.</span></span> 
     
     <span data-ttu-id="70099-179">d.</span><span class="sxs-lookup"><span data-stu-id="70099-179">d.</span></span> <span data-ttu-id="70099-180">Klikněte na tlačítko **nastavení uložit typu SAML2**.</span><span class="sxs-lookup"><span data-stu-id="70099-180">Click **Save SAML2 settings**.</span></span>

> [!TIP]
> <span data-ttu-id="70099-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="70099-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="70099-182">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="70099-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="70099-183">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="70099-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="70099-184">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="70099-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="70099-185">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="70099-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="70099-187">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70099-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="70099-188">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="70099-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="70099-190">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="70099-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="70099-192">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="70099-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="70099-194">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="70099-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="70099-196">a.</span><span class="sxs-lookup"><span data-stu-id="70099-196">a.</span></span> <span data-ttu-id="70099-197">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="70099-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="70099-198">b.</span><span class="sxs-lookup"><span data-stu-id="70099-198">b.</span></span> <span data-ttu-id="70099-199">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70099-199">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="70099-200">c.</span><span class="sxs-lookup"><span data-stu-id="70099-200">c.</span></span> <span data-ttu-id="70099-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="70099-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="70099-202">d.</span><span class="sxs-lookup"><span data-stu-id="70099-202">d.</span></span> <span data-ttu-id="70099-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="70099-203">Click **Create**.</span></span>
 
### <a name="creating-a-thirdlight-test-user"></a><span data-ttu-id="70099-204">Vytvoření zkušebního uživatele ThirdLight</span><span class="sxs-lookup"><span data-stu-id="70099-204">Creating a ThirdLight test user</span></span>

<span data-ttu-id="70099-205">Uživatelé toolog tooenable Azure AD v tooThirdLight, se musí být zřízená do ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="70099-205">tooenable Azure AD users toolog in tooThirdLight, they must be provisioned into ThirdLight.</span></span>  
<span data-ttu-id="70099-206">V případě hello ThirdLight zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="70099-206">In hello case of ThirdLight, provisioning is a manual task.</span></span>

<span data-ttu-id="70099-207">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70099-207">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="70099-208">Přihlaste se tooyour **ThirdLight** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="70099-208">Log in tooyour **ThirdLight** company site as an administrator.</span></span>

2. <span data-ttu-id="70099-209">Přejděte příliš**uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="70099-209">Go too**Users** tab.</span></span>

3. <span data-ttu-id="70099-210">Vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="70099-210">Select **Users and Groups**.</span></span>

4. <span data-ttu-id="70099-211">Klikněte na tlačítko **přidat nového uživatele** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70099-211">Click **Add new User** button.</span></span>

5. <span data-ttu-id="70099-212">Zadejte **hello uživatelské jméno, název nebo popis, e-mailu, vyberte přednastavení nebo skupiny nové členy** platného účtu AAD chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="70099-212">Enter **hello Username, Name or Description, Email, Choose a Preset or Group of New Members** of a valid AAD account you want tooprovision.</span></span>

6. <span data-ttu-id="70099-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="70099-213">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="70099-214">Můžete použít všechny ostatní Thirdlight uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Thirdlight tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="70099-214">You can use any other Thirdlight user account creation tools or APIs provided by Thirdlight tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="70099-215">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="70099-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="70099-216">V této části povolíte tak, že udělíte přístup tooThirdLight toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70099-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThirdLight.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="70099-218">**tooassign Britta Simon tooThirdLight, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70099-218">**tooassign Britta Simon tooThirdLight, perform hello following steps:**</span></span>

1. <span data-ttu-id="70099-219">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70099-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="70099-221">V seznamu aplikace hello vyberte **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="70099-221">In hello applications list, select **ThirdLight**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_app.png) 

3. <span data-ttu-id="70099-223">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="70099-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="70099-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70099-225">Click **Add** button.</span></span> <span data-ttu-id="70099-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70099-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="70099-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="70099-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="70099-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70099-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="70099-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70099-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="70099-231">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="70099-231">Testing single sign-on</span></span>

<span data-ttu-id="70099-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="70099-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="70099-233">Když kliknete na dlaždici ThirdLight hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ThirdLight aplikace.</span><span class="sxs-lookup"><span data-stu-id="70099-233">When you click hello ThirdLight tile in hello Access Panel, you should get automatically signed-on tooyour ThirdLight application.</span></span>
<span data-ttu-id="70099-234">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="70099-234">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="70099-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="70099-235">Additional resources</span></span>

* [<span data-ttu-id="70099-236">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="70099-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="70099-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="70099-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_203.png

