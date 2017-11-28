---
title: 'Kurz: Azure Active Directory integrace s Workfront | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Workfront."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: e7249b9ec769f19cf5aa7d44ff6f58705df4020a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workfront"></a><span data-ttu-id="48a8f-103">Kurz: Azure Active Directory integrace s Workfront</span><span class="sxs-lookup"><span data-stu-id="48a8f-103">Tutorial: Azure Active Directory integration with Workfront</span></span>

<span data-ttu-id="48a8f-104">V tomto kurzu zjistíte, jak toointegrate Workfront s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="48a8f-104">In this tutorial, you learn how toointegrate Workfront with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48a8f-105">Integrace Workfront s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="48a8f-105">Integrating Workfront with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="48a8f-106">Můžete řídit ve službě Azure AD, který má přístup tooWorkfront</span><span class="sxs-lookup"><span data-stu-id="48a8f-106">You can control in Azure AD who has access tooWorkfront</span></span>
- <span data-ttu-id="48a8f-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkfront (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="48a8f-107">You can enable your users tooautomatically get signed-on tooWorkfront (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48a8f-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="48a8f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="48a8f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48a8f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48a8f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="48a8f-110">Prerequisites</span></span>

<span data-ttu-id="48a8f-111">Integrace služby Azure AD s Workfront tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="48a8f-111">tooconfigure Azure AD integration with Workfront, you need hello following items:</span></span>

- <span data-ttu-id="48a8f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="48a8f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="48a8f-113">Workfront jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="48a8f-113">A Workfront single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48a8f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="48a8f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48a8f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="48a8f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48a8f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="48a8f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="48a8f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48a8f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48a8f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="48a8f-118">Scenario description</span></span>
<span data-ttu-id="48a8f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="48a8f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48a8f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="48a8f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48a8f-121">Přidání Workfront z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="48a8f-121">Adding Workfront from hello gallery</span></span>
2. <span data-ttu-id="48a8f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="48a8f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workfront-from-hello-gallery"></a><span data-ttu-id="48a8f-123">Přidání Workfront z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="48a8f-123">Adding Workfront from hello gallery</span></span>
<span data-ttu-id="48a8f-124">tooconfigure hello integrace Workfront do Azure AD, je nutné tooadd Workfront hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="48a8f-124">tooconfigure hello integration of Workfront into Azure AD, you need tooadd Workfront from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="48a8f-125">**tooadd Workfront z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48a8f-125">**tooadd Workfront from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="48a8f-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="48a8f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48a8f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="48a8f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="48a8f-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48a8f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="48a8f-133">Hello vyhledávacího pole zadejte **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-133">In hello search box, type **Workfront**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_search.png)

5. <span data-ttu-id="48a8f-135">Na panelu výsledků hello vyberte **Workfront**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="48a8f-135">In hello results panel, select **Workfront**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48a8f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="48a8f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48a8f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workfront podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="48a8f-138">In this section, you configure and test Azure AD single sign-on with Workfront based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="48a8f-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Workfront je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="48a8f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workfront is tooa user in Azure AD.</span></span> <span data-ttu-id="48a8f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Workfront musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="48a8f-140">In other words, a link relationship between an Azure AD user and hello related user in Workfront needs toobe established.</span></span>

<span data-ttu-id="48a8f-141">V Workfront, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="48a8f-141">In Workfront, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="48a8f-142">tooconfigure a testu Azure AD jednotné přihlašování s Workfront, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="48a8f-142">tooconfigure and test Azure AD single sign-on with Workfront, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="48a8f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="48a8f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="48a8f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48a8f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48a8f-145">**[Vytvoření zkušebního uživatele Workfront](#creating-a-workfront-test-user)**  -toohave protějšek Britta Simon v Workfront, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="48a8f-145">**[Creating a Workfront test user](#creating-a-workfront-test-user)** - toohave a counterpart of Britta Simon in Workfront that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="48a8f-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48a8f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48a8f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="48a8f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48a8f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="48a8f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48a8f-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Workfront.</span><span class="sxs-lookup"><span data-stu-id="48a8f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workfront application.</span></span>

<span data-ttu-id="48a8f-150">**tooconfigure Azure AD jednotné přihlašování s Workfront, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48a8f-150">**tooconfigure Azure AD single sign-on with Workfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="48a8f-151">V portálu Azure, na hello hello **Workfront** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-151">In hello Azure portal, on hello **Workfront** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="48a8f-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48a8f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_samlbase.png)

3. <span data-ttu-id="48a8f-155">Na hello **Workfront domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="48a8f-155">On hello **Workfront Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_url.png)

    <span data-ttu-id="48a8f-157">a.</span><span class="sxs-lookup"><span data-stu-id="48a8f-157">a.</span></span> <span data-ttu-id="48a8f-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.attask-ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="48a8f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.attask-ondemand.com`</span></span>

    <span data-ttu-id="48a8f-159">b.</span><span class="sxs-lookup"><span data-stu-id="48a8f-159">b.</span></span> <span data-ttu-id="48a8f-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.attasksandbox.com/SAML2`</span><span class="sxs-lookup"><span data-stu-id="48a8f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.attasksandbox.com/SAML2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="48a8f-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="48a8f-161">These values are not real.</span></span> <span data-ttu-id="48a8f-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="48a8f-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="48a8f-163">Obraťte se na [tým podpory Workfront klienta](https://www.workfront.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="48a8f-163">Contact [Workfront Client support team](https://www.workfront.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="48a8f-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="48a8f-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_certificate.png) 

5. <span data-ttu-id="48a8f-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48a8f-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="48a8f-168">Na hello **Workfront konfigurace** klikněte na tlačítko **konfigurace Workfront** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="48a8f-168">On hello **Workfront Configuration** section, click **Configure Workfront** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="48a8f-169">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="48a8f-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_configure.png) 

7. <span data-ttu-id="48a8f-171">Web společnosti Workfront tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="48a8f-171">Sign-on tooyour Workfront company site as administrator.</span></span>

8. <span data-ttu-id="48a8f-172">Přejděte příliš**jedna přihlašovací na konfigurační**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-172">Go too**Single Sign On Configuration**.</span></span>

9. <span data-ttu-id="48a8f-173">Na hello **jednotné přihlašování** dialogové okno, proveďte následující kroky hello</span><span class="sxs-lookup"><span data-stu-id="48a8f-173">On hello **Single Sign-On** dialog, perform hello following steps</span></span>
    
    ![Konfigurovat jednotné přihlašování][23]
   
    <span data-ttu-id="48a8f-175">a.</span><span class="sxs-lookup"><span data-stu-id="48a8f-175">a.</span></span> <span data-ttu-id="48a8f-176">Jako **typ**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-176">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="48a8f-177">b.</span><span class="sxs-lookup"><span data-stu-id="48a8f-177">b.</span></span> <span data-ttu-id="48a8f-178">Vyberte **služby ID zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-178">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="48a8f-179">c.</span><span class="sxs-lookup"><span data-stu-id="48a8f-179">c.</span></span> <span data-ttu-id="48a8f-180">Vložení hello **SAML jeden přihlašování adresa URL služby** do hello **adresu URL pro přihlášení portálu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="48a8f-180">Paste hello **SAML Single Sign-On Service URL** into hello **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="48a8f-181">d.</span><span class="sxs-lookup"><span data-stu-id="48a8f-181">d.</span></span> <span data-ttu-id="48a8f-182">Vložení **jednu adresu URL služby Sign-Out** do hello **Sign-Out URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="48a8f-182">Paste **Single Sign-Out Service URL** into hello **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="48a8f-183">e.</span><span class="sxs-lookup"><span data-stu-id="48a8f-183">e.</span></span> <span data-ttu-id="48a8f-184">Vložení **heslo změnit adresu URL** do hello **heslo změnit adresu URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="48a8f-184">Paste **Change Password URL** into hello **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="48a8f-185">f.</span><span class="sxs-lookup"><span data-stu-id="48a8f-185">f.</span></span> <span data-ttu-id="48a8f-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="48a8f-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="48a8f-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="48a8f-188">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="48a8f-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="48a8f-189">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="48a8f-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48a8f-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="48a8f-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="48a8f-191">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="48a8f-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="48a8f-193">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48a8f-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="48a8f-194">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="48a8f-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48a8f-196">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48a8f-198">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="48a8f-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48a8f-200">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="48a8f-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48a8f-202">a.</span><span class="sxs-lookup"><span data-stu-id="48a8f-202">a.</span></span> <span data-ttu-id="48a8f-203">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48a8f-204">b.</span><span class="sxs-lookup"><span data-stu-id="48a8f-204">b.</span></span> <span data-ttu-id="48a8f-205">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48a8f-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48a8f-206">c.</span><span class="sxs-lookup"><span data-stu-id="48a8f-206">c.</span></span> <span data-ttu-id="48a8f-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="48a8f-208">d.</span><span class="sxs-lookup"><span data-stu-id="48a8f-208">d.</span></span> <span data-ttu-id="48a8f-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-209">Click **Create**.</span></span>
 
### <a name="creating-a-workfront-test-user"></a><span data-ttu-id="48a8f-210">Vytvoření zkušebního uživatele Workfront</span><span class="sxs-lookup"><span data-stu-id="48a8f-210">Creating a Workfront test user</span></span>

<span data-ttu-id="48a8f-211">Hello cílem této části je toocreate volal Britta Simon v Workfront uživatele.</span><span class="sxs-lookup"><span data-stu-id="48a8f-211">hello objective of this section is toocreate a user called Britta Simon in Workfront.</span></span>

<span data-ttu-id="48a8f-212">**toocreate uživatel volal Britta Simon v Workfront, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48a8f-212">**toocreate a user called Britta Simon in Workfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="48a8f-213">Přihlaste se na tooyour Workfront společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="48a8f-213">Sign on tooyour Workfront company site as administrator.</span></span>
2. <span data-ttu-id="48a8f-214">V nabídce hello hello nahoře, klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-214">In hello menu on hello top, click **People**.</span></span>
3. <span data-ttu-id="48a8f-215">Klikněte na tlačítko **nové osobě**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-215">Click **New Person**.</span></span> 
4. <span data-ttu-id="48a8f-216">V dialogovém okně hello nové osobě proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="48a8f-216">On hello New Person dialog, perform hello following steps:</span></span>
   
    ![Vytvořit uživatele s Workfront testu][21] 
   
    <span data-ttu-id="48a8f-218">a.</span><span class="sxs-lookup"><span data-stu-id="48a8f-218">a.</span></span> <span data-ttu-id="48a8f-219">V hello **křestní jméno** textovému poli, zadejte "Britta."</span><span class="sxs-lookup"><span data-stu-id="48a8f-219">In hello **First Name** textbox, type "Britta."</span></span>
   
    <span data-ttu-id="48a8f-220">b.</span><span class="sxs-lookup"><span data-stu-id="48a8f-220">b.</span></span> <span data-ttu-id="48a8f-221">V hello **příjmení** textovému poli, zadejte "Simon."</span><span class="sxs-lookup"><span data-stu-id="48a8f-221">In hello **Last Name** textbox, type "Simon."</span></span>
   
    <span data-ttu-id="48a8f-222">c.</span><span class="sxs-lookup"><span data-stu-id="48a8f-222">c.</span></span> <span data-ttu-id="48a8f-223">V hello **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu Britta Simon v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="48a8f-223">In hello **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="48a8f-224">d.</span><span class="sxs-lookup"><span data-stu-id="48a8f-224">d.</span></span> <span data-ttu-id="48a8f-225">Klikněte na tlačítko **přidat osobu**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-225">Click **Add Person**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="48a8f-226">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="48a8f-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="48a8f-227">V této části povolíte tak, že udělíte přístup tooWorkfront toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48a8f-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkfront.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="48a8f-229">**tooassign Britta Simon tooWorkfront, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48a8f-229">**tooassign Britta Simon tooWorkfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="48a8f-230">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="48a8f-232">V seznamu aplikace hello vyberte **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-232">In hello applications list, select **Workfront**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_app.png) 

3. <span data-ttu-id="48a8f-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="48a8f-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="48a8f-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48a8f-236">Click **Add** button.</span></span> <span data-ttu-id="48a8f-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48a8f-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="48a8f-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="48a8f-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="48a8f-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48a8f-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48a8f-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48a8f-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48a8f-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="48a8f-242">Testing single sign-on</span></span>

<span data-ttu-id="48a8f-243">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="48a8f-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="48a8f-244">Když kliknete na dlaždici Workfront hello v hello přístupového panelu, měli byste obdržet přihlašovací stránku Workfront aplikace.</span><span class="sxs-lookup"><span data-stu-id="48a8f-244">When you click hello Workfront tile in hello Access Panel, you should get login page of Workfront application.</span></span>
<span data-ttu-id="48a8f-245">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="48a8f-245">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="48a8f-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="48a8f-246">Additional resources</span></span>

* [<span data-ttu-id="48a8f-247">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48a8f-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48a8f-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="48a8f-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_04.png
[21]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_08.png
[23]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_06.png
[100]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_203.png

