---
title: "Kurz: Azure Active Directory integrace s centrální Cerner | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Cerner střed."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 3493d180e8f229b7cd228769f780f10208114889
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a><span data-ttu-id="304c7-103">Kurz: Azure Active Directory integrace s Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="304c7-103">Tutorial: Azure Active Directory integration with Cerner Central</span></span>

<span data-ttu-id="304c7-104">V tomto kurzu zjistíte, jak toointegrate střed Cerner s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="304c7-104">In this tutorial, you learn how toointegrate Cerner Central with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="304c7-105">Integrace střed Cerner s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="304c7-105">Integrating Cerner Central with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="304c7-106">Můžete řídit ve službě Azure AD, který má přístup tooCerner – střed</span><span class="sxs-lookup"><span data-stu-id="304c7-106">You can control in Azure AD who has access tooCerner Central</span></span>
- <span data-ttu-id="304c7-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCerner střed (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="304c7-107">You can enable your users tooautomatically get signed-on tooCerner Central (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="304c7-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="304c7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="304c7-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="304c7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="304c7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="304c7-110">Prerequisites</span></span>

<span data-ttu-id="304c7-111">Integrace služby Azure AD s centrální Cerner tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="304c7-111">tooconfigure Azure AD integration with Cerner Central, you need hello following items:</span></span>

- <span data-ttu-id="304c7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="304c7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="304c7-113">Schválené Cerner centrální systému účtu</span><span class="sxs-lookup"><span data-stu-id="304c7-113">An approved Cerner Central System Account</span></span>

> [!NOTE]
> <span data-ttu-id="304c7-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="304c7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="304c7-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="304c7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="304c7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="304c7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="304c7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="304c7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="304c7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="304c7-118">Scenario description</span></span>
<span data-ttu-id="304c7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="304c7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="304c7-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="304c7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="304c7-121">Přidání Cerner střed z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="304c7-121">Adding Cerner Central from hello gallery</span></span>
2. <span data-ttu-id="304c7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="304c7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cerner-central-from-hello-gallery"></a><span data-ttu-id="304c7-123">Přidání Cerner střed z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="304c7-123">Adding Cerner Central from hello gallery</span></span>
<span data-ttu-id="304c7-124">tooconfigure hello integrace Cerner střední do Azure AD, je nutné tooadd Cerner střed hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="304c7-124">tooconfigure hello integration of Cerner Central into Azure AD, you need tooadd Cerner Central from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="304c7-125">**tooadd Cerner střed z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="304c7-125">**tooadd Cerner Central from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="304c7-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="304c7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="304c7-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="304c7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="304c7-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="304c7-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="304c7-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítka v dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="304c7-131">tooadd new application, click **New application** button on top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="304c7-133">Hello vyhledávacího pole zadejte **Cerner střed**.</span><span class="sxs-lookup"><span data-stu-id="304c7-133">In hello search box, type **Cerner Central**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_search.png)

5. <span data-ttu-id="304c7-135">Na panelu výsledků hello vyberte **Cerner střed**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="304c7-135">In hello results panel, select **Cerner Central**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="304c7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="304c7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="304c7-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s centrální Cerner podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="304c7-138">In this section, you configure and test Azure AD single sign-on with Cerner Central based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="304c7-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello in – střed Cerner je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="304c7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cerner Central is tooa user in Azure AD.</span></span> <span data-ttu-id="304c7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello in – střed Cerner musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="304c7-140">In other words, a link relationship between an Azure AD user and hello related user in Cerner Central needs toobe established.</span></span>

<span data-ttu-id="304c7-141">tooconfigure a testu Azure AD jednotné přihlašování s centrální Cerner, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="304c7-141">tooconfigure and test Azure AD single sign-on with Cerner Central, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="304c7-142">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="304c7-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="304c7-143">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="304c7-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="304c7-144">**[Vytvoření zkušebního uživatele střed Cerner](#creating-a-cerner-central-test-user)**  -toohave protějšek Britta Simon in – střed Cerner, která je propojená toohello Azure AD reprezentace hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="304c7-144">**[Creating a Cerner Central test user](#creating-a-cerner-central-test-user)** - toohave a counterpart of Britta Simon in Cerner Central that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="304c7-145">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="304c7-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="304c7-146">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="304c7-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="304c7-147">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="304c7-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="304c7-148">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Cerner střed.</span><span class="sxs-lookup"><span data-stu-id="304c7-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cerner Central application.</span></span>

<span data-ttu-id="304c7-149">**tooconfigure Azure AD jednotné přihlašování s Cerner – střed, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="304c7-149">**tooconfigure Azure AD single sign-on with Cerner Central, perform hello following steps:**</span></span>

1. <span data-ttu-id="304c7-150">V portálu Azure, na hello hello **Cerner střed** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="304c7-150">In hello Azure portal, on hello **Cerner Central** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="304c7-152">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="304c7-152">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. <span data-ttu-id="304c7-154">Na hello **Cerner centrální domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="304c7-154">On hello **Cerner Central Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_url.png)

    <span data-ttu-id="304c7-156">a.</span><span class="sxs-lookup"><span data-stu-id="304c7-156">a.</span></span> <span data-ttu-id="304c7-157">V hello **identifikátor** textovému poli, typ hello hodnotu pomocí hello následující vzorce:</span><span class="sxs-lookup"><span data-stu-id="304c7-157">In hello **Identifier** textbox, type hello value using hello following patterns:</span></span>
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcerner.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |

    <span data-ttu-id="304c7-158">b.</span><span class="sxs-lookup"><span data-stu-id="304c7-158">b.</span></span> <span data-ttu-id="304c7-159">V hello **adresa URL odpovědi** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="304c7-159">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://cernercentral.com/<instasncename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://<subdomain>.sandboxcernercentral.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="304c7-160">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="304c7-160">These values are not hello real.</span></span> <span data-ttu-id="304c7-161">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="304c7-161">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="304c7-162">Obraťte se na [tým podpory střed Cerner](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="304c7-162">Contact [Cerner Central support team](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) tooget these values.</span></span>
 
4. <span data-ttu-id="304c7-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="304c7-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="304c7-165">toogenerate hello **Metadata** adresu url, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="304c7-165">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="304c7-166">a.</span><span class="sxs-lookup"><span data-stu-id="304c7-166">a.</span></span> <span data-ttu-id="304c7-167">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="304c7-167">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appregistrations.png)
   
    <span data-ttu-id="304c7-169">b.</span><span class="sxs-lookup"><span data-stu-id="304c7-169">b.</span></span> <span data-ttu-id="304c7-170">Klikněte na tlačítko **koncové body** tooopen **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="304c7-170">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpointicon.png)

    <span data-ttu-id="304c7-172">c.</span><span class="sxs-lookup"><span data-stu-id="304c7-172">c.</span></span> <span data-ttu-id="304c7-173">Klikněte na tlačítko hello kopie tlačítko toocopy **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="304c7-173">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpoint.png)
     
    <span data-ttu-id="304c7-175">d.</span><span class="sxs-lookup"><span data-stu-id="304c7-175">d.</span></span> <span data-ttu-id="304c7-176">Nyní přejděte na stránce vlastností toohello **Cerner střed** a kopírování hello **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="304c7-176">Now go toohello property page of **Cerner Central** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appid.png)

    <span data-ttu-id="304c7-178">e.</span><span class="sxs-lookup"><span data-stu-id="304c7-178">e.</span></span> <span data-ttu-id="304c7-179">Generovat hello **adresu URL metadat** pomocí hello následující vzoru:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="304c7-179">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

6. <span data-ttu-id="304c7-180">tooconfigure jednotného přihlašování na **Cerner střed** postranní, budete potřebovat toosend hello **adresu URL metadat** příliš[Cerner střed podporu](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span><span class="sxs-lookup"><span data-stu-id="304c7-180">tooconfigure single sign-on on **Cerner Central** side, you need toosend hello **Metadata URL** too[Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span></span> <span data-ttu-id="304c7-181">Hello jednotného přihlašování je potřeba nakonfigurovat na integraci aplikací straně toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="304c7-181">They configure hello SSO on application side toocomplete hello integration.</span></span>

> [!TIP]
> <span data-ttu-id="304c7-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="304c7-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="304c7-183">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="304c7-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="304c7-184">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="304c7-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="304c7-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="304c7-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="304c7-186">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="304c7-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span> 

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="304c7-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="304c7-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="304c7-189">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="304c7-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="304c7-191">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="304c7-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="304c7-193">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="304c7-193">tooopen hello **User** dialog, click **Add**.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="304c7-195">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="304c7-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="304c7-197">a.</span><span class="sxs-lookup"><span data-stu-id="304c7-197">a.</span></span> <span data-ttu-id="304c7-198">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="304c7-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="304c7-199">b.</span><span class="sxs-lookup"><span data-stu-id="304c7-199">b.</span></span> <span data-ttu-id="304c7-200">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="304c7-200">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="304c7-201">c.</span><span class="sxs-lookup"><span data-stu-id="304c7-201">c.</span></span> <span data-ttu-id="304c7-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="304c7-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="304c7-203">d.</span><span class="sxs-lookup"><span data-stu-id="304c7-203">d.</span></span> <span data-ttu-id="304c7-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="304c7-204">Click **Create**.</span></span>
 
### <a name="creating-a-cerner-central-test-user"></a><span data-ttu-id="304c7-205">Vytvoření zkušebního uživatele Cerner – střed</span><span class="sxs-lookup"><span data-stu-id="304c7-205">Creating a Cerner Central test user</span></span>

<span data-ttu-id="304c7-206">**Střed Cerner** aplikace umožňuje ověřování z kteréhokoli zprostředkovatele federovaných identit.</span><span class="sxs-lookup"><span data-stu-id="304c7-206">**Cerner Central** application allows authentication from any federated identity provider.</span></span> <span data-ttu-id="304c7-207">Pokud je uživatel moct toolog toohello aplikace domovské stránce, jsou federovaný a mít pro jakékoli ruční zřizování není nutné.</span><span class="sxs-lookup"><span data-stu-id="304c7-207">If a user is able toolog in toohello application home page, they are federated and have no need for any manual provisioning.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="304c7-208">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="304c7-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="304c7-209">V této části povolíte tak, že udělíte přístup tooCerner střed Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="304c7-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCerner Central.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="304c7-211">**tooassign tooCerner Britta Simon – střed, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="304c7-211">**tooassign Britta Simon tooCerner Central, perform hello following steps:**</span></span>

1. <span data-ttu-id="304c7-212">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="304c7-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="304c7-214">V seznamu aplikace hello vyberte **Cerner střed**.</span><span class="sxs-lookup"><span data-stu-id="304c7-214">In hello applications list, select **Cerner Central**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_app.png) 

3. <span data-ttu-id="304c7-216">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="304c7-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="304c7-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="304c7-218">Click **Add** button.</span></span> <span data-ttu-id="304c7-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="304c7-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="304c7-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="304c7-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="304c7-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="304c7-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="304c7-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="304c7-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="304c7-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="304c7-224">Testing single sign-on</span></span>

<span data-ttu-id="304c7-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="304c7-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="304c7-226">Po kliknutí na tlačítko hello Cerner střed dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Cerner střed aplikace.</span><span class="sxs-lookup"><span data-stu-id="304c7-226">When you click hello Cerner Central tile in hello Access Panel, you should get automatically signed-on tooyour Cerner Central application.</span></span> <span data-ttu-id="304c7-227">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="304c7-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="304c7-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="304c7-228">Additional resources</span></span>

* [<span data-ttu-id="304c7-229">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="304c7-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="304c7-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="304c7-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_203.png

