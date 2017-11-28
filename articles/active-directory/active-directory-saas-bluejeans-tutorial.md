---
title: 'Kurz: Azure Active Directory integrace s BlueJeans | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a BlueJeans."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 67613303a9f854afbf4619418cc1607d329caf94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a><span data-ttu-id="1cd0f-103">Kurz: Azure Active Directory integrace s BlueJeans</span><span class="sxs-lookup"><span data-stu-id="1cd0f-103">Tutorial: Azure Active Directory integration with BlueJeans</span></span>

<span data-ttu-id="1cd0f-104">V tomto kurzu zjistíte, jak toointegrate BlueJeans s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1cd0f-104">In this tutorial, you learn how toointegrate BlueJeans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1cd0f-105">Integrace BlueJeans s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-105">Integrating BlueJeans with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1cd0f-106">Můžete řídit ve službě Azure AD, který má přístup tooBlueJeans</span><span class="sxs-lookup"><span data-stu-id="1cd0f-106">You can control in Azure AD who has access tooBlueJeans</span></span>
- <span data-ttu-id="1cd0f-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBlueJeans (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1cd0f-107">You can enable your users tooautomatically get signed-on tooBlueJeans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1cd0f-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1cd0f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1cd0f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1cd0f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cd0f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1cd0f-110">Prerequisites</span></span>

<span data-ttu-id="1cd0f-111">Integrace služby Azure AD s BlueJeans tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-111">tooconfigure Azure AD integration with BlueJeans, you need hello following items:</span></span>

- <span data-ttu-id="1cd0f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1cd0f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1cd0f-113">BlueJeans jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1cd0f-113">A BlueJeans single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1cd0f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1cd0f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1cd0f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1cd0f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1cd0f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1cd0f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1cd0f-118">Scenario description</span></span>
<span data-ttu-id="1cd0f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1cd0f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1cd0f-121">Přidání BlueJeans z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1cd0f-121">Adding BlueJeans from hello gallery</span></span>
2. <span data-ttu-id="1cd0f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1cd0f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bluejeans-from-hello-gallery"></a><span data-ttu-id="1cd0f-123">Přidání BlueJeans z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1cd0f-123">Adding BlueJeans from hello gallery</span></span>
<span data-ttu-id="1cd0f-124">tooconfigure hello integrace BlueJeans do Azure AD, je nutné tooadd BlueJeans hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-124">tooconfigure hello integration of BlueJeans into Azure AD, you need tooadd BlueJeans from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1cd0f-125">**tooadd BlueJeans z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1cd0f-125">**tooadd BlueJeans from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1cd0f-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1cd0f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1cd0f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1cd0f-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1cd0f-133">Hello vyhledávacího pole zadejte **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-133">In hello search box, type **BlueJeans**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. <span data-ttu-id="1cd0f-135">Na panelu výsledků hello vyberte **BlueJeans**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-135">In hello results panel, select **BlueJeans**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1cd0f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1cd0f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1cd0f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BlueJeans podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="1cd0f-138">In this section, you configure and test Azure AD single sign-on with BlueJeans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1cd0f-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v BlueJeans je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BlueJeans is tooa user in Azure AD.</span></span> <span data-ttu-id="1cd0f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v BlueJeans musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-140">In other words, a link relationship between an Azure AD user and hello related user in BlueJeans needs toobe established.</span></span>

<span data-ttu-id="1cd0f-141">V BlueJeans, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-141">In BlueJeans, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1cd0f-142">tooconfigure a testu Azure AD jednotné přihlašování s BlueJeans, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-142">tooconfigure and test Azure AD single sign-on with BlueJeans, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1cd0f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1cd0f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1cd0f-145">**[Vytvoření zkušebního uživatele BlueJeans](#creating-a-bluejeans-test-user)**  -toohave protějšek Britta Simon v BlueJeans, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-145">**[Creating a BlueJeans test user](#creating-a-bluejeans-test-user)** - toohave a counterpart of Britta Simon in BlueJeans that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1cd0f-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1cd0f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1cd0f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1cd0f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1cd0f-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BlueJeans application.</span></span>

<span data-ttu-id="1cd0f-150">**tooconfigure Azure AD jednotné přihlašování s BlueJeans, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1cd0f-150">**tooconfigure Azure AD single sign-on with BlueJeans, perform hello following steps:**</span></span>

1. <span data-ttu-id="1cd0f-151">V portálu Azure, na hello hello **BlueJeans** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-151">In hello Azure portal, on hello **BlueJeans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1cd0f-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. <span data-ttu-id="1cd0f-155">Na hello **BlueJeans domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-155">On hello **BlueJeans Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    <span data-ttu-id="1cd0f-157">a.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-157">a.</span></span> <span data-ttu-id="1cd0f-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="1cd0f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    <span data-ttu-id="1cd0f-159">b.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-159">b.</span></span> <span data-ttu-id="1cd0f-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="1cd0f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1cd0f-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-161">These values are not real.</span></span> <span data-ttu-id="1cd0f-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1cd0f-163">Obraťte se na [tým podpory BlueJeans klienta](https://support.bluejeans.com/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-163">Contact [BlueJeans Client support team](https://support.bluejeans.com/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="1cd0f-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. <span data-ttu-id="1cd0f-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1cd0f-168">Na hello **BlueJeans konfigurace** klikněte na tlačítko **konfigurace BlueJeans** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-168">On hello **BlueJeans Configuration** section, click **Configure BlueJeans** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1cd0f-169">Kopírování hello **Sign-Out adresu URL, adresy URL pro změnu hesla a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1cd0f-169">Copy hello **Sign-Out URL, Change Password URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. <span data-ttu-id="1cd0f-171">V okně prohlížeče jiných webových přihlásit tooyour **BlueJeans** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-171">In a different web browser window, log in tooyour **BlueJeans** company site as an administrator.</span></span>

8. <span data-ttu-id="1cd0f-172">Přejděte příliš**správce \> nastavení skupiny \> zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-172">Go too**ADMIN \> Group Settings \> Security**.</span></span>
   
   <span data-ttu-id="1cd0f-173">![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "správce")</span><span class="sxs-lookup"><span data-stu-id="1cd0f-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span></span>

9. <span data-ttu-id="1cd0f-174">V hello **zabezpečení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-174">In hello **Security** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="1cd0f-175">![Jednotné přihlašování SAML na](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "jednotného přihlašování k SAML")</span><span class="sxs-lookup"><span data-stu-id="1cd0f-175">![SAML Single Sign On](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML Single Sign On")</span></span>   
   
   <span data-ttu-id="1cd0f-176">a.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-176">a.</span></span> <span data-ttu-id="1cd0f-177">Vyberte **jednotného přihlašování k SAML**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-177">Select **SAML Single Sign On**.</span></span>
  
   <span data-ttu-id="1cd0f-178">b.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-178">b.</span></span> <span data-ttu-id="1cd0f-179">Vyberte **zapněte automatické zřizování**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-179">Select **Enable automatic provisioning**.</span></span>

10. <span data-ttu-id="1cd0f-180">Přesunout s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-180">Move on with hello following steps:</span></span>

    <span data-ttu-id="1cd0f-181">![Certifikát cesta](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "certifikátu cesta")</span><span class="sxs-lookup"><span data-stu-id="1cd0f-181">![Certificate Path](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificate Path")</span></span>
    
    <span data-ttu-id="1cd0f-182">a.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-182">a.</span></span> <span data-ttu-id="1cd0f-183">Klikněte na tlačítko **zvolit soubor**a pak nahrajte hello stáhnout certifikát.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-183">Click **Choose File**, and then upload hello downloaded certificate.</span></span>
   
    <span data-ttu-id="1cd0f-184">b.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-184">b.</span></span> <span data-ttu-id="1cd0f-185">Vložení **SAML jeden přihlašování adresa URL služby** do hello **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-185">Paste **SAML Single Sign-On Service URL** into hello **Login URL** textbox.</span></span>
   
    <span data-ttu-id="1cd0f-186">c.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-186">c.</span></span> <span data-ttu-id="1cd0f-187">Vložení **heslo změnit adresu URL** do hello **heslo změnit adresu URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-187">Paste **Change Password URL** into hello **Password Change URL** textbox.</span></span>
   
    <span data-ttu-id="1cd0f-188">d.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-188">d.</span></span> <span data-ttu-id="1cd0f-189">Vložení **Sign-Out URL** do hello **adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-189">Paste **Sign-Out URL** into hello **Logout URL** textbox.</span></span>

11. <span data-ttu-id="1cd0f-190">Přesunout s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-190">Move on with hello following steps:</span></span>
    
    <span data-ttu-id="1cd0f-191">![Uložit změny](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "uložit změny")</span><span class="sxs-lookup"><span data-stu-id="1cd0f-191">![Save Changes](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Save Changes")</span></span>
    
    <span data-ttu-id="1cd0f-192">a.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-192">a.</span></span> <span data-ttu-id="1cd0f-193">V hello **id uživatele** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-193">In hello **User id** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="1cd0f-194">b.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-194">b.</span></span> <span data-ttu-id="1cd0f-195">V hello **e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-195">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="1cd0f-196">c.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-196">c.</span></span> <span data-ttu-id="1cd0f-197">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-197">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="1cd0f-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="1cd0f-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1cd0f-199">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1cd0f-200">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1cd0f-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1cd0f-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1cd0f-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="1cd0f-202">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1cd0f-204">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1cd0f-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1cd0f-205">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1cd0f-207">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1cd0f-209">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1cd0f-211">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1cd0f-213">a.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-213">a.</span></span> <span data-ttu-id="1cd0f-214">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1cd0f-215">b.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-215">b.</span></span> <span data-ttu-id="1cd0f-216">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1cd0f-217">c.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-217">c.</span></span> <span data-ttu-id="1cd0f-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1cd0f-219">d.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-219">d.</span></span> <span data-ttu-id="1cd0f-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-220">Click **Create**.</span></span>
 
### <a name="creating-a-bluejeans-test-user"></a><span data-ttu-id="1cd0f-221">Vytvoření zkušebního uživatele BlueJeans</span><span class="sxs-lookup"><span data-stu-id="1cd0f-221">Creating a BlueJeans test user</span></span>

<span data-ttu-id="1cd0f-222">Uživatelé toolog tooenable Azure AD v tooBlueJeans, se musí být zřízená do BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-222">tooenable Azure AD users toolog in tooBlueJeans, they must be provisioned into BlueJeans.</span></span>  

<span data-ttu-id="1cd0f-223">V případě BlueJeans zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-223">In case of BlueJeans, provisioning is a manual task.</span></span>

<span data-ttu-id="1cd0f-224">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1cd0f-224">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="1cd0f-225">Přihlaste se tooyour **BlueJeans** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-225">Log in tooyour **BlueJeans** company site as an administrator.</span></span>

2. <span data-ttu-id="1cd0f-226">Přejděte příliš**správce \> spravovat uživatele \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-226">Go too**ADMIN \> Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="1cd0f-227">![Správce](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "správce")</span><span class="sxs-lookup"><span data-stu-id="1cd0f-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span></span>
   
   >[!IMPORTANT]
   ><span data-ttu-id="1cd0f-228">Hello **přidat uživatele** karta je dostupná pouze v případě, v hello **karta zabezpečení**, **zapněte automatické zřizování** není zaškrtnuta.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-228">hello **Add User** tab is only available if, in hello **Security tab**, **Enable automatic provisioning** is unchecked.</span></span> 
   
3. <span data-ttu-id="1cd0f-229">V hello **přidat uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1cd0f-229">In hello **Add User** section, perform hello following steps:</span></span>

    <span data-ttu-id="1cd0f-230">![Přidat uživatele](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="1cd0f-230">![Add User](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Add User")</span></span>
    
    <span data-ttu-id="1cd0f-231">a.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-231">a.</span></span> <span data-ttu-id="1cd0f-232">Zadejte **uživatelské jméno BlueJeans**, **e-mailová adresa**, **BlueJeans schůzku ID**, **moderátora hesla**, **úplný název** , hello **společnosti** z platný účet AAD chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-232">Type a **BlueJeans Username**, an **Email address**, a **BlueJeans Meeting ID**, a **Moderator Passcode**, a **Full Name**, hello **Company** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
    
    <span data-ttu-id="1cd0f-233">b.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-233">b.</span></span> <span data-ttu-id="1cd0f-234">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-234">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="1cd0f-235">Můžete použít všechny ostatní BlueJeans uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované BlueJeans tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-235">You can use any other BlueJeans user account creation tools or APIs provided by BlueJeans tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1cd0f-236">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="1cd0f-236">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1cd0f-237">V této části povolíte tak, že udělíte přístup tooBlueJeans toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-237">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlueJeans.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1cd0f-239">**tooassign Britta Simon tooBlueJeans, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1cd0f-239">**tooassign Britta Simon tooBlueJeans, perform hello following steps:**</span></span>

1. <span data-ttu-id="1cd0f-240">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-240">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1cd0f-242">V seznamu aplikace hello vyberte **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-242">In hello applications list, select **BlueJeans**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. <span data-ttu-id="1cd0f-244">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-244">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1cd0f-246">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-246">Click **Add** button.</span></span> <span data-ttu-id="1cd0f-247">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1cd0f-249">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-249">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1cd0f-250">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1cd0f-251">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1cd0f-252">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1cd0f-252">Testing single sign-on</span></span>

<span data-ttu-id="1cd0f-253">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-253">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1cd0f-254">Po kliknutí na tlačítko hello BlueJeans dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku BlueJeans aplikace.</span><span class="sxs-lookup"><span data-stu-id="1cd0f-254">When you click hello BlueJeans tile in hello Access Panel, you should get login page of BlueJeans application.</span></span>
<span data-ttu-id="1cd0f-255">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1cd0f-255">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1cd0f-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1cd0f-256">Additional resources</span></span>

* [<span data-ttu-id="1cd0f-257">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1cd0f-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1cd0f-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1cd0f-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

