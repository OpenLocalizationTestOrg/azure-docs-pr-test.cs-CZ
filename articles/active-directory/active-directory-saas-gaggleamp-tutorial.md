---
title: 'Kurz: Azure Active Directory integrace s GaggleAMP | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a GaggleAMP."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9761019a79f935b6695a5ae1214c256c887df457
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a><span data-ttu-id="c575e-103">Kurz: Azure Active Directory integrace s GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="c575e-103">Tutorial: Azure Active Directory integration with GaggleAMP</span></span>

<span data-ttu-id="c575e-104">V tomto kurzu zjistíte, jak toointegrate GaggleAMP s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c575e-104">In this tutorial, you learn how toointegrate GaggleAMP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c575e-105">Integrace GaggleAMP s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c575e-105">Integrating GaggleAMP with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c575e-106">Můžete řídit ve službě Azure AD, který má přístup tooGaggleAMP</span><span class="sxs-lookup"><span data-stu-id="c575e-106">You can control in Azure AD who has access tooGaggleAMP</span></span>
- <span data-ttu-id="c575e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGaggleAMP (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c575e-107">You can enable your users tooautomatically get signed-on tooGaggleAMP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c575e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c575e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c575e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c575e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c575e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c575e-110">Prerequisites</span></span>

<span data-ttu-id="c575e-111">Integrace služby Azure AD s GaggleAMP tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c575e-111">tooconfigure Azure AD integration with GaggleAMP, you need hello following items:</span></span>

- <span data-ttu-id="c575e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c575e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c575e-113">GaggleAMP jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c575e-113">A GaggleAMP single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c575e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c575e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c575e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c575e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c575e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c575e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c575e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c575e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c575e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c575e-118">Scenario description</span></span>
<span data-ttu-id="c575e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c575e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c575e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c575e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c575e-121">Přidání GaggleAMP z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c575e-121">Adding GaggleAMP from hello gallery</span></span>
2. <span data-ttu-id="c575e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c575e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gaggleamp-from-hello-gallery"></a><span data-ttu-id="c575e-123">Přidání GaggleAMP z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c575e-123">Adding GaggleAMP from hello gallery</span></span>
<span data-ttu-id="c575e-124">tooconfigure hello integrace GaggleAMP do Azure AD, je nutné tooadd GaggleAMP hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c575e-124">tooconfigure hello integration of GaggleAMP into Azure AD, you need tooadd GaggleAMP from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c575e-125">**tooadd GaggleAMP z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c575e-125">**tooadd GaggleAMP from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c575e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c575e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c575e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c575e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c575e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c575e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c575e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c575e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c575e-133">Hello vyhledávacího pole zadejte **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="c575e-133">In hello search box, type **GaggleAMP**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_search.png)

5. <span data-ttu-id="c575e-135">Na panelu výsledků hello vyberte **GaggleAMP**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c575e-135">In hello results panel, select **GaggleAMP**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c575e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c575e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c575e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s GaggleAMP podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c575e-138">In this section, you configure and test Azure AD single sign-on with GaggleAMP based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c575e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v GaggleAMP je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c575e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in GaggleAMP is tooa user in Azure AD.</span></span> <span data-ttu-id="c575e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v GaggleAMP musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c575e-140">In other words, a link relationship between an Azure AD user and hello related user in GaggleAMP needs toobe established.</span></span>

<span data-ttu-id="c575e-141">V GaggleAMP, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c575e-141">In GaggleAMP, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c575e-142">tooconfigure a testu Azure AD jednotné přihlašování s GaggleAMP, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c575e-142">tooconfigure and test Azure AD single sign-on with GaggleAMP, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c575e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c575e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c575e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c575e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c575e-145">**[Vytvoření zkušebního uživatele GaggleAMP](#creating-a-gaggleamp-test-user)**  -toohave protějšek Britta Simon v GaggleAMP, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c575e-145">**[Creating a GaggleAMP test user](#creating-a-gaggleamp-test-user)** - toohave a counterpart of Britta Simon in GaggleAMP that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c575e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c575e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c575e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c575e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c575e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c575e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c575e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="c575e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your GaggleAMP application.</span></span>

<span data-ttu-id="c575e-150">**tooconfigure Azure AD jednotné přihlašování s GaggleAMP, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c575e-150">**tooconfigure Azure AD single sign-on with GaggleAMP, perform hello following steps:**</span></span>

1. <span data-ttu-id="c575e-151">V portálu Azure, na hello hello **GaggleAMP** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c575e-151">In hello Azure portal, on hello **GaggleAMP** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c575e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c575e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

3. <span data-ttu-id="c575e-155">Na hello **GaggleAMP domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c575e-155">On hello **GaggleAMP Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     <span data-ttu-id="c575e-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.gaggleamp.com`</span><span class="sxs-lookup"><span data-stu-id="c575e-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.gaggleamp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c575e-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="c575e-158">hello value is not real.</span></span> <span data-ttu-id="c575e-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c575e-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="c575e-160">Obraťte se na [tým podpory GaggleAMP klienta](mailto:sales@gaggleamp.com) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c575e-160">Contact [GaggleAMP Client support team](mailto:sales@gaggleamp.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="c575e-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c575e-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

5. <span data-ttu-id="c575e-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c575e-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c575e-165">Na hello **GaggleAMP konfigurace** klikněte na tlačítko **konfigurace GaggleAMP** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c575e-165">On hello **GaggleAMP Configuration** section, click **Configure GaggleAMP** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c575e-166">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c575e-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

7. <span data-ttu-id="c575e-168">V jiné instanci prohlížeče, přejděte toohello jednotné přihlašování SAML stránky vytvořené pro vám hello Gaggle podporu team (například: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span><span class="sxs-lookup"><span data-stu-id="c575e-168">In another browser instance, navigate toohello SAML SSO page created for you by hello Gaggle support team (for example: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span></span>

8. <span data-ttu-id="c575e-169">Na vaše **jednotné přihlašování SAML** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c575e-169">On your **SAML SSO** page, perform hello following steps:</span></span>  
   
    ![GaggleAMP jednotné přihlášení](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png) 
 
    <span data-ttu-id="c575e-171">a.</span><span class="sxs-lookup"><span data-stu-id="c575e-171">a.</span></span> <span data-ttu-id="c575e-172">V hello **vystavitele zprostředkovatele Identity** textovému poli, vložte hodnotu hello **URL vystavitele** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c575e-172">In hello **Identity Provider Issuer** textbox, paste hello value of **Issuer URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="c575e-173">b.</span><span class="sxs-lookup"><span data-stu-id="c575e-173">b.</span></span> <span data-ttu-id="c575e-174">V hello **Identity zprostředkovatele jeden přihlašovací adresa URL** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c575e-174">In hello **Identity Provider Single Sign-On URL** textbox, paste hello  value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="c575e-175">c.</span><span class="sxs-lookup"><span data-stu-id="c575e-175">c.</span></span> <span data-ttu-id="c575e-176">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="c575e-176">Click **Save**</span></span>      

    <span data-ttu-id="c575e-177">d.</span><span class="sxs-lookup"><span data-stu-id="c575e-177">d.</span></span> <span data-ttu-id="c575e-178">Odeslat hello **certifikátu (Base64)** certifikátu tooyour [tým podpory GaggleAMP](mailto:sales@gaggleamp.com).</span><span class="sxs-lookup"><span data-stu-id="c575e-178">Send hello **Certificate (Base64)** certificate tooyour [GaggleAMP support team](mailto:sales@gaggleamp.com).</span></span>

> [!TIP]
> <span data-ttu-id="c575e-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c575e-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c575e-180">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c575e-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c575e-181">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c575e-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c575e-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c575e-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="c575e-183">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c575e-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c575e-185">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c575e-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c575e-186">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c575e-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c575e-188">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c575e-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c575e-190">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c575e-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c575e-192">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c575e-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c575e-194">a.</span><span class="sxs-lookup"><span data-stu-id="c575e-194">a.</span></span> <span data-ttu-id="c575e-195">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c575e-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c575e-196">b.</span><span class="sxs-lookup"><span data-stu-id="c575e-196">b.</span></span> <span data-ttu-id="c575e-197">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c575e-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c575e-198">c.</span><span class="sxs-lookup"><span data-stu-id="c575e-198">c.</span></span> <span data-ttu-id="c575e-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c575e-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c575e-200">d.</span><span class="sxs-lookup"><span data-stu-id="c575e-200">d.</span></span> <span data-ttu-id="c575e-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c575e-201">Click **Create**.</span></span>
 
### <a name="creating-a-gaggleamp-test-user"></a><span data-ttu-id="c575e-202">Vytvoření zkušebního uživatele GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="c575e-202">Creating a GaggleAMP test user</span></span>

<span data-ttu-id="c575e-203">Hello cílem této části je toocreate volal Britta Simon v GaggleAMP uživatele.</span><span class="sxs-lookup"><span data-stu-id="c575e-203">hello objective of this section is toocreate a user called Britta Simon in GaggleAMP.</span></span> <span data-ttu-id="c575e-204">GaggleAMP podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="c575e-204">GaggleAMP supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="c575e-205">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="c575e-205">There is no action item for you in this section.</span></span> <span data-ttu-id="c575e-206">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="c575e-206">A new user is created during an attempt tooaccess GaggleAMP if it doesn't exist yet.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c575e-207">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c575e-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c575e-208">V této části povolíte tak, že udělíte přístup tooGaggleAMP toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c575e-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGaggleAMP.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c575e-210">**tooassign Britta Simon tooGaggleAMP, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c575e-210">**tooassign Britta Simon tooGaggleAMP, perform hello following steps:**</span></span>

1. <span data-ttu-id="c575e-211">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c575e-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c575e-213">V seznamu aplikace hello vyberte **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="c575e-213">In hello applications list, select **GaggleAMP**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

3. <span data-ttu-id="c575e-215">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c575e-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c575e-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c575e-217">Click **Add** button.</span></span> <span data-ttu-id="c575e-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c575e-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c575e-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c575e-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c575e-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c575e-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c575e-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c575e-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c575e-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c575e-223">Testing single sign-on</span></span>

<span data-ttu-id="c575e-224">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="c575e-224">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="c575e-225">Když kliknete na dlaždici GaggleAMP hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour GaggleAMP aplikace.</span><span class="sxs-lookup"><span data-stu-id="c575e-225">When you click hello GaggleAMP tile in hello Access Panel, you should get automatically signed-on tooyour GaggleAMP application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c575e-226">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c575e-226">Additional resources</span></span>

* [<span data-ttu-id="c575e-227">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c575e-227">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c575e-228">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c575e-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png

