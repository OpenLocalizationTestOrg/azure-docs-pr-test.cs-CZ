---
title: "Kurz: Azure Active Directory integrace s přiblížení | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a přiblížení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 623a1f428ad1f0aa2c8205b79d61720cad5fc6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a><span data-ttu-id="22b16-103">Kurz: Azure Active Directory integrace s přiblížení</span><span class="sxs-lookup"><span data-stu-id="22b16-103">Tutorial: Azure Active Directory integration with Zoom</span></span>

<span data-ttu-id="22b16-104">V tomto kurzu zjistíte, jak toointegrate zvětšení v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="22b16-104">In this tutorial, you learn how toointegrate Zoom with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="22b16-105">Integrace přiblížení s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="22b16-105">Integrating Zoom with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="22b16-106">Můžete ovládat ve službě Azure AD, který má přístup tooZoom.</span><span class="sxs-lookup"><span data-stu-id="22b16-106">You can control in Azure AD who has access tooZoom.</span></span>
- <span data-ttu-id="22b16-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZoom (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22b16-107">You can enable your users tooautomatically get signed-on tooZoom (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="22b16-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22b16-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="22b16-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="22b16-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22b16-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="22b16-110">Prerequisites</span></span>

<span data-ttu-id="22b16-111">integrace tooconfigure Azure AD s přiblížení, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="22b16-111">tooconfigure Azure AD integration with Zoom, you need hello following items:</span></span>

- <span data-ttu-id="22b16-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="22b16-112">An Azure AD subscription</span></span>
- <span data-ttu-id="22b16-113">Přiblížení jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="22b16-113">A Zoom single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="22b16-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="22b16-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="22b16-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="22b16-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="22b16-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="22b16-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="22b16-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="22b16-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="22b16-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="22b16-118">Scenario description</span></span>
<span data-ttu-id="22b16-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="22b16-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="22b16-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="22b16-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="22b16-121">Přidání přiblížení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="22b16-121">Adding Zoom from hello gallery</span></span>
2. <span data-ttu-id="22b16-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="22b16-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoom-from-hello-gallery"></a><span data-ttu-id="22b16-123">Přidání přiblížení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="22b16-123">Adding Zoom from hello gallery</span></span>
<span data-ttu-id="22b16-124">integrace hello tooconfigure přiblížení či oddálení do služby Azure AD, je nutné tooadd přiblížení hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="22b16-124">tooconfigure hello integration of Zoom into Azure AD, you need tooadd Zoom from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="22b16-125">**tooadd přiblížení z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22b16-125">**tooadd Zoom from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="22b16-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="22b16-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="22b16-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="22b16-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="22b16-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="22b16-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="22b16-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22b16-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="22b16-133">Hello vyhledávacího pole zadejte **zvětšení**, vyberte **zvětšení** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="22b16-133">In hello search box, type **Zoom**, select **Zoom** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Zvětšení v seznamu výsledků hello](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="22b16-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="22b16-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="22b16-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s přiblížení podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="22b16-136">In this section, you configure and test Azure AD single sign-on with Zoom based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="22b16-137">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v přiblížení je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22b16-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zoom is tooa user in Azure AD.</span></span> <span data-ttu-id="22b16-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v toobe přiblížení musí navázat.</span><span class="sxs-lookup"><span data-stu-id="22b16-138">In other words, a link relationship between an Azure AD user and hello related user in Zoom needs toobe established.</span></span>

<span data-ttu-id="22b16-139">V přiblížení, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="22b16-139">In Zoom, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="22b16-140">tooconfigure a testu Azure AD jednotné přihlašování s přiblížení, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="22b16-140">tooconfigure and test Azure AD single sign-on with Zoom, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="22b16-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="22b16-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="22b16-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22b16-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="22b16-143">**[Vytvoření zkušebního uživatele přiblížení](#create-a-zoom-test-user)**  -toohave protějšek Britta Simon v přiblížení, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="22b16-143">**[Create a Zoom test user](#create-a-zoom-test-user)** - toohave a counterpart of Britta Simon in Zoom that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="22b16-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22b16-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="22b16-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="22b16-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="22b16-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="22b16-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="22b16-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci přiblížení.</span><span class="sxs-lookup"><span data-stu-id="22b16-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zoom application.</span></span>

<span data-ttu-id="22b16-148">**tooconfigure Azure AD jednotné přihlašování s přiblížení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22b16-148">**tooconfigure Azure AD single sign-on with Zoom, perform hello following steps:**</span></span>

1. <span data-ttu-id="22b16-149">V portálu Azure, na hello hello **zvětšení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="22b16-149">In hello Azure portal, on hello **Zoom** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="22b16-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22b16-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_samlbase.png)

3. <span data-ttu-id="22b16-153">Na hello **zvětšení domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="22b16-153">On hello **Zoom Domain and URLs** section, perform hello following steps:</span></span>

    ![Přiblížení domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_url.png)

    <span data-ttu-id="22b16-155">a.</span><span class="sxs-lookup"><span data-stu-id="22b16-155">a.</span></span> <span data-ttu-id="22b16-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="22b16-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.zoom.us`</span></span>

    <span data-ttu-id="22b16-157">b.</span><span class="sxs-lookup"><span data-stu-id="22b16-157">b.</span></span> <span data-ttu-id="22b16-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="22b16-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.zoom.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="22b16-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="22b16-159">These values are not real.</span></span> <span data-ttu-id="22b16-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="22b16-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="22b16-161">Obraťte se na [tým podpory zvětšení klienta](https://support.zoom.us/hc) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="22b16-161">Contact [Zoom Client support team](https://support.zoom.us/hc) tooget these values.</span></span> 
 
4. <span data-ttu-id="22b16-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="22b16-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_certificate.png) 

5. <span data-ttu-id="22b16-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22b16-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-zoom-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="22b16-166">Na hello **zvětšení konfigurace** klikněte na tlačítko **konfigurace zvětšení** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="22b16-166">On hello **Zoom Configuration** section, click **Configure Zoom** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="22b16-167">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="22b16-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace přiblížení](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_configure.png) 

7. <span data-ttu-id="22b16-169">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour přiblížení.</span><span class="sxs-lookup"><span data-stu-id="22b16-169">In a different web browser window, log in tooyour Zoom company site as an administrator.</span></span>

8. <span data-ttu-id="22b16-170">Klikněte na tlačítko hello **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="22b16-170">Click hello **Single Sign-On** tab.</span></span>
   
    <span data-ttu-id="22b16-171">![Karta přihlašování](./media/active-directory-saas-zoom-tutorial/IC784700.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="22b16-171">![Single sign-on tab](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single sign-on")</span></span>

9. <span data-ttu-id="22b16-172">Klikněte na tlačítko hello **řízení zabezpečení** kartě, a potom přejděte toohello **jednotné přihlašování** nastavení.</span><span class="sxs-lookup"><span data-stu-id="22b16-172">Click hello **Security Control** tab, and then go toohello **Single Sign-On** settings.</span></span>

10. <span data-ttu-id="22b16-173">V hello části jednotné přihlašování proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22b16-173">In hello Single Sign-On section, perform hello following steps:</span></span>
   
    <span data-ttu-id="22b16-174">![Jednotné přihlašování v části](./media/active-directory-saas-zoom-tutorial/IC784701.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="22b16-174">![Single sign-on section](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single sign-on")</span></span>
   
    <span data-ttu-id="22b16-175">a.</span><span class="sxs-lookup"><span data-stu-id="22b16-175">a.</span></span> <span data-ttu-id="22b16-176">V hello **přihlašovací adresa URL stránky** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22b16-176">In hello **Sign-in page URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="22b16-177">b.</span><span class="sxs-lookup"><span data-stu-id="22b16-177">b.</span></span> <span data-ttu-id="22b16-178">V hello **adresy URL odhlašovací stránky** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22b16-178">In hello **Sign-out page URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
     
    <span data-ttu-id="22b16-179">c.</span><span class="sxs-lookup"><span data-stu-id="22b16-179">c.</span></span> <span data-ttu-id="22b16-180">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="22b16-180">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity provider certificate** textbox.</span></span>

    <span data-ttu-id="22b16-181">d.</span><span class="sxs-lookup"><span data-stu-id="22b16-181">d.</span></span> <span data-ttu-id="22b16-182">V hello **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22b16-182">In hello **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="22b16-183">e.</span><span class="sxs-lookup"><span data-stu-id="22b16-183">e.</span></span> <span data-ttu-id="22b16-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="22b16-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="22b16-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="22b16-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="22b16-186">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="22b16-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="22b16-187">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="22b16-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="22b16-188">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="22b16-188">Create an Azure AD test user</span></span>

<span data-ttu-id="22b16-189">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="22b16-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="22b16-191">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22b16-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="22b16-192">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22b16-192">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-zoom-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="22b16-194">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="22b16-194">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-zoom-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="22b16-196">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22b16-196">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-zoom-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="22b16-198">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="22b16-198">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-zoom-tutorial/create_aaduser_04.png)

    <span data-ttu-id="22b16-200">a.</span><span class="sxs-lookup"><span data-stu-id="22b16-200">a.</span></span> <span data-ttu-id="22b16-201">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="22b16-201">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="22b16-202">b.</span><span class="sxs-lookup"><span data-stu-id="22b16-202">b.</span></span> <span data-ttu-id="22b16-203">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22b16-203">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="22b16-204">c.</span><span class="sxs-lookup"><span data-stu-id="22b16-204">c.</span></span> <span data-ttu-id="22b16-205">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="22b16-205">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="22b16-206">d.</span><span class="sxs-lookup"><span data-stu-id="22b16-206">d.</span></span> <span data-ttu-id="22b16-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="22b16-207">Click **Create**.</span></span>
 
### <a name="create-a-zoom-test-user"></a><span data-ttu-id="22b16-208">Vytvoření zkušebního uživatele přiblížení</span><span class="sxs-lookup"><span data-stu-id="22b16-208">Create a Zoom test user</span></span>

<span data-ttu-id="22b16-209">V pořadí tooenable Azure AD Uživatelé toolog v tooZoom musí být zřízená do přiblížení.</span><span class="sxs-lookup"><span data-stu-id="22b16-209">In order tooenable Azure AD users toolog in tooZoom, they must be provisioned into Zoom.</span></span> <span data-ttu-id="22b16-210">V případě hello přiblížení či oddálení zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="22b16-210">In hello case of Zoom, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="22b16-211">tooprovision uživatelský účet, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="22b16-211">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="22b16-212">Přihlaste se tooyour **zvětšení** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="22b16-212">Log in tooyour **Zoom** company site as an administrator.</span></span>
 
2. <span data-ttu-id="22b16-213">Klikněte na tlačítko hello **správy účtů** a pak klikněte **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="22b16-213">Click hello **Account Management** tab, and then click **User Management**.</span></span>

3. <span data-ttu-id="22b16-214">V části Správa uživatelů hello, klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="22b16-214">In hello User Management section, click **Add users**.</span></span>
   
    <span data-ttu-id="22b16-215">![Správa uživatelů](./media/active-directory-saas-zoom-tutorial/IC784703.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="22b16-215">![User management](./media/active-directory-saas-zoom-tutorial/IC784703.png "User management")</span></span>

4. <span data-ttu-id="22b16-216">Na hello **přidat uživatele** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22b16-216">On hello **Add users** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="22b16-217">![Přidání uživatelů](./media/active-directory-saas-zoom-tutorial/IC784704.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="22b16-217">![Add users](./media/active-directory-saas-zoom-tutorial/IC784704.png "Add users")</span></span>
   
    <span data-ttu-id="22b16-218">a.</span><span class="sxs-lookup"><span data-stu-id="22b16-218">a.</span></span> <span data-ttu-id="22b16-219">Jako **typ uživatele**, vyberte **základní**.</span><span class="sxs-lookup"><span data-stu-id="22b16-219">As **User Type**, select **Basic**.</span></span>

    <span data-ttu-id="22b16-220">b.</span><span class="sxs-lookup"><span data-stu-id="22b16-220">b.</span></span> <span data-ttu-id="22b16-221">V hello **e-mailů** textovému poli, typ hello e-mailovou adresu platný Azure AD účet chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="22b16-221">In hello **Emails** textbox, type hello email address of a valid Azure AD account you want tooprovision.</span></span>

    <span data-ttu-id="22b16-222">c.</span><span class="sxs-lookup"><span data-stu-id="22b16-222">c.</span></span> <span data-ttu-id="22b16-223">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="22b16-223">Click **Add**.</span></span>

> [!NOTE]
> <span data-ttu-id="22b16-224">Můžete použít jakékoli jiné přiblížení uživatel účet vytvoření nástroje nebo rozhraní API poskytované přiblížení tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="22b16-224">You can use any other Zoom user account creation tools or APIs provided by Zoom tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="22b16-225">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="22b16-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="22b16-226">V této části povolíte tak, že udělíte přístup tooZoom toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22b16-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZoom.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="22b16-228">**tooassign Britta Simon tooZoom, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22b16-228">**tooassign Britta Simon tooZoom, perform hello following steps:**</span></span>

1. <span data-ttu-id="22b16-229">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="22b16-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="22b16-231">V seznamu aplikace hello vyberte **zvětšení**.</span><span class="sxs-lookup"><span data-stu-id="22b16-231">In hello applications list, select **Zoom**.</span></span>

    ![Hello přiblížení odkaz v seznamu aplikace hello](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_app.png)  

3. <span data-ttu-id="22b16-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="22b16-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="22b16-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22b16-235">Click **Add** button.</span></span> <span data-ttu-id="22b16-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22b16-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="22b16-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="22b16-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="22b16-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22b16-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="22b16-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22b16-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="22b16-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="22b16-241">Test single sign-on</span></span>

<span data-ttu-id="22b16-242">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="22b16-242">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="22b16-243">Když kliknete na dlaždici přiblížení hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour přiblížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="22b16-243">When you click hello Zoom tile in hello Access Panel, you should get automatically signed-on tooyour Zoom application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22b16-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="22b16-244">Additional resources</span></span>

* [<span data-ttu-id="22b16-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="22b16-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="22b16-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="22b16-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_203.png

