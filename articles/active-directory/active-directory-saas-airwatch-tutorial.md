---
title: 'Kurz: Azure Active Directory integrace s AirWatch | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a AirWatch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e5230d5a36824778a4d9804dadf9f13a0d11a68d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a><span data-ttu-id="c58e5-103">Kurz: Azure Active Directory integrace s AirWatch</span><span class="sxs-lookup"><span data-stu-id="c58e5-103">Tutorial: Azure Active Directory integration with AirWatch</span></span>

<span data-ttu-id="c58e5-104">V tomto kurzu zjistíte, jak toointegrate AirWatch s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c58e5-104">In this tutorial, you learn how toointegrate AirWatch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c58e5-105">Integrace AirWatch s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c58e5-105">Integrating AirWatch with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c58e5-106">Můžete řídit ve službě Azure AD, který má přístup tooAirWatch</span><span class="sxs-lookup"><span data-stu-id="c58e5-106">You can control in Azure AD who has access tooAirWatch</span></span>
- <span data-ttu-id="c58e5-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAirWatch (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c58e5-107">You can enable your users tooautomatically get signed-on tooAirWatch (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c58e5-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c58e5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c58e5-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c58e5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c58e5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c58e5-110">Prerequisites</span></span>

<span data-ttu-id="c58e5-111">Integrace služby Azure AD s AirWatch tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c58e5-111">tooconfigure Azure AD integration with AirWatch, you need hello following items:</span></span>

- <span data-ttu-id="c58e5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c58e5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c58e5-113">AirWatch jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c58e5-113">An AirWatch single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c58e5-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c58e5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c58e5-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c58e5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c58e5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c58e5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c58e5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c58e5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c58e5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c58e5-118">Scenario description</span></span>
<span data-ttu-id="c58e5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c58e5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c58e5-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c58e5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c58e5-121">Přidání AirWatch z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c58e5-121">Adding AirWatch from hello gallery</span></span>
2. <span data-ttu-id="c58e5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c58e5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-airwatch-from-hello-gallery"></a><span data-ttu-id="c58e5-123">Přidání AirWatch z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c58e5-123">Adding AirWatch from hello gallery</span></span>
<span data-ttu-id="c58e5-124">tooconfigure hello integrace AirWatch do Azure AD, je nutné tooadd AirWatch hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c58e5-124">tooconfigure hello integration of AirWatch into Azure AD, you need tooadd AirWatch from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c58e5-125">**tooadd AirWatch z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c58e5-125">**tooadd AirWatch from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c58e5-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c58e5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c58e5-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c58e5-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c58e5-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c58e5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c58e5-133">Hello vyhledávacího pole zadejte **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-133">In hello search box, type **AirWatch**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. <span data-ttu-id="c58e5-135">Na panelu výsledků hello vyberte **AirWatch**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c58e5-135">In hello results panel, select **AirWatch**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c58e5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c58e5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c58e5-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s AirWatch podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c58e5-138">In this section, you configure and test Azure AD single sign-on with AirWatch based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c58e5-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v AirWatch je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c58e5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AirWatch is tooa user in Azure AD.</span></span> <span data-ttu-id="c58e5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v AirWatch musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c58e5-140">In other words, a link relationship between an Azure AD user and hello related user in AirWatch needs toobe established.</span></span>

<span data-ttu-id="c58e5-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v AirWatch.</span><span class="sxs-lookup"><span data-stu-id="c58e5-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in AirWatch.</span></span>

<span data-ttu-id="c58e5-142">tooconfigure a testu Azure AD jednotné přihlašování s AirWatch, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c58e5-142">tooconfigure and test Azure AD single sign-on with AirWatch, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c58e5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c58e5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c58e5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c58e5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c58e5-145">**[Vytvoření zkušebního uživatele AirWatch](#creating-a-airwatch-test-user)**  -toohave protějšek Britta Simon v AirWatch, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c58e5-145">**[Creating a AirWatch test user](#creating-a-airwatch-test-user)** - toohave a counterpart of Britta Simon in AirWatch that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c58e5-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c58e5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c58e5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c58e5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c58e5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c58e5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c58e5-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci AirWatch.</span><span class="sxs-lookup"><span data-stu-id="c58e5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AirWatch application.</span></span>

<span data-ttu-id="c58e5-150">**tooconfigure Azure AD jednotné přihlašování s AirWatch, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c58e5-150">**tooconfigure Azure AD single sign-on with AirWatch, perform hello following steps:**</span></span>

1. <span data-ttu-id="c58e5-151">V portálu Azure, na hello hello **AirWatch** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-151">In hello Azure portal, on hello **AirWatch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c58e5-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c58e5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. <span data-ttu-id="c58e5-155">Na hello **AirWatch domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c58e5-155">On hello **AirWatch Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    <span data-ttu-id="c58e5-157">a.</span><span class="sxs-lookup"><span data-stu-id="c58e5-157">a.</span></span> <span data-ttu-id="c58e5-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span><span class="sxs-lookup"><span data-stu-id="c58e5-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span></span>

    <span data-ttu-id="c58e5-159">b.</span><span class="sxs-lookup"><span data-stu-id="c58e5-159">b.</span></span> <span data-ttu-id="c58e5-160">V hello **identifikátor** textovému poli, hodnota typu hello jako`AirWatch`</span><span class="sxs-lookup"><span data-stu-id="c58e5-160">In hello **Identifier** textbox, type hello value as `AirWatch`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c58e5-161">Tato hodnota není skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="c58e5-161">This value is not hello real.</span></span> <span data-ttu-id="c58e5-162">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c58e5-162">Update this value with hello actual Sign-on URL.</span></span> <span data-ttu-id="c58e5-163">Obraťte se na [tým podpory AirWatch klienta](http://www.air-watch.com/company/contact-us/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c58e5-163">Contact [AirWatch Client support team](http://www.air-watch.com/company/contact-us/) tooget this value.</span></span> 
 
4. <span data-ttu-id="c58e5-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c58e5-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. <span data-ttu-id="c58e5-166">Na hello **AirWatch konfigurace** klikněte na tlačítko **konfigurace AirWatch** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c58e5-166">On hello **AirWatch Configuration** section, click **Configure AirWatch** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c58e5-167">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c58e5-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. <span data-ttu-id="c58e5-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c58e5-169">Click **Save** button.</span></span>

    <span data-ttu-id="c58e5-170">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="c58e5-170">![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span></span>
7. <span data-ttu-id="c58e5-171">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti AirWatch tooyour.</span><span class="sxs-lookup"><span data-stu-id="c58e5-171">In a different web browser window, log in tooyour AirWatch company site as an administrator.</span></span>

8. <span data-ttu-id="c58e5-172">V levém navigačním podokně hello, klikněte na **účty**a potom klikněte na **správci**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-172">In hello left navigation pane, click **Accounts**, and then click **Administrators**.</span></span>
   
   <span data-ttu-id="c58e5-173">![Správci](./media/active-directory-saas-airwatch-tutorial/ic791920.png "správci")</span><span class="sxs-lookup"><span data-stu-id="c58e5-173">![Administrators](./media/active-directory-saas-airwatch-tutorial/ic791920.png "Administrators")</span></span>

9. <span data-ttu-id="c58e5-174">Rozbalte hello **nastavení** nabídce a pak klikněte na tlačítko **Directory Services**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-174">Expand hello **Settings** menu, and then click **Directory Services**.</span></span>
   
   <span data-ttu-id="c58e5-175">![Nastavení](./media/active-directory-saas-airwatch-tutorial/ic791921.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="c58e5-175">![Settings](./media/active-directory-saas-airwatch-tutorial/ic791921.png "Settings")</span></span>

10. <span data-ttu-id="c58e5-176">Klikněte na tlačítko hello **uživatele** na kartě hello **Base DN** textovému poli, zadejte název domény a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-176">Click hello **User** tab, in hello **Base DN** textbox, type your domain name, and then click **Save**.</span></span>
   
   <span data-ttu-id="c58e5-177">![Uživatel](./media/active-directory-saas-airwatch-tutorial/ic791922.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="c58e5-177">![User](./media/active-directory-saas-airwatch-tutorial/ic791922.png "User")</span></span>

11. <span data-ttu-id="c58e5-178">Klikněte na tlačítko hello **Server** kartě.</span><span class="sxs-lookup"><span data-stu-id="c58e5-178">Click hello **Server** tab.</span></span>
   
   <span data-ttu-id="c58e5-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "serveru")</span><span class="sxs-lookup"><span data-stu-id="c58e5-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span></span>

12. <span data-ttu-id="c58e5-180">Proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c58e5-180">Perform hello following steps:</span></span>
    
    <span data-ttu-id="c58e5-181">![Nahrát](./media/active-directory-saas-airwatch-tutorial/ic791924.png "nahrát")</span><span class="sxs-lookup"><span data-stu-id="c58e5-181">![Upload](./media/active-directory-saas-airwatch-tutorial/ic791924.png "Upload")</span></span>   
    
    <span data-ttu-id="c58e5-182">a.</span><span class="sxs-lookup"><span data-stu-id="c58e5-182">a.</span></span> <span data-ttu-id="c58e5-183">Jako **typu adresáře**, vyberte **žádné**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-183">As **Directory Type**, select **None**.</span></span>

    <span data-ttu-id="c58e5-184">b.</span><span class="sxs-lookup"><span data-stu-id="c58e5-184">b.</span></span> <span data-ttu-id="c58e5-185">Vyberte **pomocí SAML pro ověřování**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-185">Select **Use SAML For Authentication**.</span></span>

    <span data-ttu-id="c58e5-186">c.</span><span class="sxs-lookup"><span data-stu-id="c58e5-186">c.</span></span> <span data-ttu-id="c58e5-187">tooupload hello stažený certifikát, klikněte na tlačítko **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-187">tooupload hello downloaded certificate, click **Upload**.</span></span>

13. <span data-ttu-id="c58e5-188">V hello **požadavku** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c58e5-188">In hello **Request** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="c58e5-189">![Žádosti o](./media/active-directory-saas-airwatch-tutorial/ic791925.png "požadavku")</span><span class="sxs-lookup"><span data-stu-id="c58e5-189">![Request](./media/active-directory-saas-airwatch-tutorial/ic791925.png "Request")</span></span>  

    <span data-ttu-id="c58e5-190">a.</span><span class="sxs-lookup"><span data-stu-id="c58e5-190">a.</span></span> <span data-ttu-id="c58e5-191">Jako **typ vazby žádosti**, vyberte **POST**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-191">As **Request Binding Type**, select **POST**.</span></span>

    <span data-ttu-id="c58e5-192">b.</span><span class="sxs-lookup"><span data-stu-id="c58e5-192">b.</span></span> <span data-ttu-id="c58e5-193">V portálu Azure, na hello hello **nakonfigurovat jednotné přihlašování v Airwatch** dialogové okno stránky, kopie hello **SAML jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **zprostředkovatele Identity Jednotné přihlašování na adresu URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c58e5-193">In hello Azure portal, on hello **Configure single sign-on at Airwatch** dialog page, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **Identity Provider Single Sign On URL** textbox.</span></span>

    <span data-ttu-id="c58e5-194">c.</span><span class="sxs-lookup"><span data-stu-id="c58e5-194">c.</span></span> <span data-ttu-id="c58e5-195">Jako **NameID formátu**, vyberte **e-mailovou adresu**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-195">As **NameID Format**, select **Email Address**.</span></span>

    <span data-ttu-id="c58e5-196">d.</span><span class="sxs-lookup"><span data-stu-id="c58e5-196">d.</span></span> <span data-ttu-id="c58e5-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-197">Click **Save**.</span></span>

14. <span data-ttu-id="c58e5-198">Klikněte na tlačítko hello **uživatele** kartě znovu.</span><span class="sxs-lookup"><span data-stu-id="c58e5-198">Click hello **User** tab again.</span></span>
    
    <span data-ttu-id="c58e5-199">![Uživatel](./media/active-directory-saas-airwatch-tutorial/ic791926.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="c58e5-199">![User](./media/active-directory-saas-airwatch-tutorial/ic791926.png "User")</span></span>

15. <span data-ttu-id="c58e5-200">V hello **atribut** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c58e5-200">In hello **Attribute** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="c58e5-201">![Atribut](./media/active-directory-saas-airwatch-tutorial/ic791927.png "atribut")</span><span class="sxs-lookup"><span data-stu-id="c58e5-201">![Attribute](./media/active-directory-saas-airwatch-tutorial/ic791927.png "Attribute")</span></span>

    <span data-ttu-id="c58e5-202">a.</span><span class="sxs-lookup"><span data-stu-id="c58e5-202">a.</span></span> <span data-ttu-id="c58e5-203">V hello **identifikátor objektu** textovému poli, typ **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-203">In hello **Object Identifier** textbox, type **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span></span>

    <span data-ttu-id="c58e5-204">b.</span><span class="sxs-lookup"><span data-stu-id="c58e5-204">b.</span></span> <span data-ttu-id="c58e5-205">V hello **uživatelské jméno** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-205">In hello **Username** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="c58e5-206">c.</span><span class="sxs-lookup"><span data-stu-id="c58e5-206">c.</span></span> <span data-ttu-id="c58e5-207">V hello **zobrazovaný název** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-207">In hello **Display Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="c58e5-208">d.</span><span class="sxs-lookup"><span data-stu-id="c58e5-208">d.</span></span> <span data-ttu-id="c58e5-209">V hello **křestní jméno** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-209">In hello **First Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="c58e5-210">e.</span><span class="sxs-lookup"><span data-stu-id="c58e5-210">e.</span></span> <span data-ttu-id="c58e5-211">V hello **příjmení** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-211">In hello **Last Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

    <span data-ttu-id="c58e5-212">f.</span><span class="sxs-lookup"><span data-stu-id="c58e5-212">f.</span></span> <span data-ttu-id="c58e5-213">V hello **e-mailu** textovému poli, typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-213">In hello **Email** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="c58e5-214">g.</span><span class="sxs-lookup"><span data-stu-id="c58e5-214">g.</span></span> <span data-ttu-id="c58e5-215">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-215">Click **Save**.</span></span>

<CE>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c58e5-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c58e5-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="c58e5-217">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c58e5-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c58e5-219">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c58e5-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c58e5-220">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c58e5-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c58e5-222">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c58e5-224">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c58e5-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c58e5-226">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c58e5-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c58e5-228">a.</span><span class="sxs-lookup"><span data-stu-id="c58e5-228">a.</span></span> <span data-ttu-id="c58e5-229">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c58e5-230">b.</span><span class="sxs-lookup"><span data-stu-id="c58e5-230">b.</span></span> <span data-ttu-id="c58e5-231">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c58e5-231">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="c58e5-232">c.</span><span class="sxs-lookup"><span data-stu-id="c58e5-232">c.</span></span> <span data-ttu-id="c58e5-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c58e5-234">d.</span><span class="sxs-lookup"><span data-stu-id="c58e5-234">d.</span></span> <span data-ttu-id="c58e5-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-235">Click **Create**.</span></span>
 
### <a name="creating-a-airwatch-test-user"></a><span data-ttu-id="c58e5-236">Vytvoření zkušebního uživatele AirWatch</span><span class="sxs-lookup"><span data-stu-id="c58e5-236">Creating a AirWatch test user</span></span>

<span data-ttu-id="c58e5-237">Uživatelé toolog tooenable Azure AD v tooAirWatch, se musí být zřízená v tooAirWatch.</span><span class="sxs-lookup"><span data-stu-id="c58e5-237">tooenable Azure AD users toolog in tooAirWatch, they must be provisioned in tooAirWatch.</span></span>

* <span data-ttu-id="c58e5-238">Při zřizování AirWatch, je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="c58e5-238">When AirWatch, provisioning is a manual task.</span></span>

<span data-ttu-id="c58e5-239">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c58e5-239">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="c58e5-240">Přihlaste se tooyour **AirWatch** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c58e5-240">Log in tooyour **AirWatch** company site as administrator.</span></span>
2. <span data-ttu-id="c58e5-241">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **účty**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-241">In hello navigation pane on hello left side, click **Accounts**, and then click **Users**.</span></span>
   
   <span data-ttu-id="c58e5-242">![Uživatelé](./media/active-directory-saas-airwatch-tutorial/ic791929.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="c58e5-242">![Users](./media/active-directory-saas-airwatch-tutorial/ic791929.png "Users")</span></span>
3. <span data-ttu-id="c58e5-243">V hello **uživatelé** nabídky, klikněte na tlačítko **zobrazení seznamu**a potom klikněte na **přidat \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-243">In hello **Users** menu, click **List View**, and then click **Add \> Add User**.</span></span>
   
   <span data-ttu-id="c58e5-244">![Přidat uživatele](./media/active-directory-saas-airwatch-tutorial/ic791930.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="c58e5-244">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791930.png "Add User")</span></span>
4. <span data-ttu-id="c58e5-245">Na hello **přidat nebo upravit uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c58e5-245">On hello **Add / Edit User** dialog, perform hello following steps:</span></span>

   <span data-ttu-id="c58e5-246">![Přidat uživatele](./media/active-directory-saas-airwatch-tutorial/ic791931.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="c58e5-246">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791931.png "Add User")</span></span>   
   1. <span data-ttu-id="c58e5-247">Typ hello **uživatelské jméno**, **heslo**, **Potvrdit heslo**, **křestní jméno**, **příjmení**,  **E-mailová adresa** platný Azure souvisejícím s účtem služby Active Directory do hello chcete tooprovision textových polí.</span><span class="sxs-lookup"><span data-stu-id="c58e5-247">Type hello **Username**, **Password**, **Confirm Password**, **First Name**, **Last Name**, **Email Address** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   2. <span data-ttu-id="c58e5-248">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-248">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="c58e5-249">Můžete použít všechny ostatní AirWatch uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované AirWatch tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c58e5-249">You can use any other AirWatch user account creation tools or APIs provided by AirWatch tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c58e5-250">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c58e5-250">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c58e5-251">V této části povolíte tak, že udělíte přístup tooAirWatch toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c58e5-251">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAirWatch.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c58e5-253">**tooassign Britta Simon tooAirWatch, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c58e5-253">**tooassign Britta Simon tooAirWatch, perform hello following steps:**</span></span>

1. <span data-ttu-id="c58e5-254">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-254">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c58e5-256">V seznamu aplikace hello vyberte **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-256">In hello applications list, select **AirWatch**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. <span data-ttu-id="c58e5-258">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c58e5-258">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c58e5-260">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c58e5-260">Click **Add** button.</span></span> <span data-ttu-id="c58e5-261">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c58e5-261">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c58e5-263">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c58e5-263">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c58e5-264">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c58e5-264">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c58e5-265">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c58e5-265">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c58e5-266">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c58e5-266">Testing single sign-on</span></span>

<span data-ttu-id="c58e5-267">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c58e5-267">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c58e5-268">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c58e5-268">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="c58e5-269">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c58e5-269">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c58e5-270">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c58e5-270">Additional resources</span></span>

* [<span data-ttu-id="c58e5-271">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c58e5-271">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c58e5-272">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c58e5-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

