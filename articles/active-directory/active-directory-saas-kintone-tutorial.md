---
title: 'Kurz: Azure Active Directory integrace s Kintone | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Kintone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 4f61fb95c643743504699b175beeff06e01c95c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a><span data-ttu-id="20583-103">Kurz: Azure Active Directory integrace s Kintone</span><span class="sxs-lookup"><span data-stu-id="20583-103">Tutorial: Azure Active Directory integration with Kintone</span></span>

<span data-ttu-id="20583-104">V tomto kurzu zjistíte, jak toointegrate Kintone s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="20583-104">In this tutorial, you learn how toointegrate Kintone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="20583-105">Integrace Kintone s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="20583-105">Integrating Kintone with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="20583-106">Můžete řídit ve službě Azure AD, který má přístup tooKintone</span><span class="sxs-lookup"><span data-stu-id="20583-106">You can control in Azure AD who has access tooKintone</span></span>
- <span data-ttu-id="20583-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooKintone (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="20583-107">You can enable your users tooautomatically get signed-on tooKintone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="20583-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="20583-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="20583-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="20583-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20583-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="20583-110">Prerequisites</span></span>

<span data-ttu-id="20583-111">Integrace služby Azure AD s Kintone tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="20583-111">tooconfigure Azure AD integration with Kintone, you need hello following items:</span></span>

- <span data-ttu-id="20583-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="20583-112">An Azure AD subscription</span></span>
- <span data-ttu-id="20583-113">Kintone jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="20583-113">A Kintone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="20583-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="20583-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="20583-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="20583-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="20583-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="20583-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="20583-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="20583-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="20583-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="20583-118">Scenario description</span></span>
<span data-ttu-id="20583-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="20583-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="20583-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="20583-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="20583-121">Přidání Kintone z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="20583-121">Adding Kintone from hello gallery</span></span>
2. <span data-ttu-id="20583-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="20583-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kintone-from-hello-gallery"></a><span data-ttu-id="20583-123">Přidání Kintone z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="20583-123">Adding Kintone from hello gallery</span></span>
<span data-ttu-id="20583-124">tooconfigure hello integrace Kintone do Azure AD, je nutné tooadd Kintone hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="20583-124">tooconfigure hello integration of Kintone into Azure AD, you need tooadd Kintone from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="20583-125">**tooadd Kintone z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="20583-125">**tooadd Kintone from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="20583-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="20583-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="20583-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="20583-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="20583-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="20583-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="20583-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20583-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="20583-133">Hello vyhledávacího pole zadejte **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="20583-133">In hello search box, type **Kintone**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_search.png)

5. <span data-ttu-id="20583-135">Na panelu výsledků hello vyberte **Kintone**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="20583-135">In hello results panel, select **Kintone**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="20583-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="20583-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="20583-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kintone podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="20583-138">In this section, you configure and test Azure AD single sign-on with Kintone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="20583-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Kintone je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20583-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kintone is tooa user in Azure AD.</span></span> <span data-ttu-id="20583-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Kintone musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="20583-140">In other words, a link relationship between an Azure AD user and hello related user in Kintone needs toobe established.</span></span>

<span data-ttu-id="20583-141">V Kintone, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="20583-141">In Kintone, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="20583-142">tooconfigure a testu Azure AD jednotné přihlašování s Kintone, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="20583-142">tooconfigure and test Azure AD single sign-on with Kintone, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="20583-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="20583-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="20583-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20583-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="20583-145">**[Vytvoření zkušebního uživatele Kintone](#creating-a-kintone-test-user)**  -toohave protějšek Britta Simon v Kintone, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="20583-145">**[Creating a Kintone test user](#creating-a-kintone-test-user)** - toohave a counterpart of Britta Simon in Kintone that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="20583-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="20583-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="20583-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="20583-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="20583-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="20583-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="20583-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Kintone.</span><span class="sxs-lookup"><span data-stu-id="20583-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kintone application.</span></span>

<span data-ttu-id="20583-150">**tooconfigure Azure AD jednotné přihlašování s Kintone, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="20583-150">**tooconfigure Azure AD single sign-on with Kintone, perform hello following steps:**</span></span>

1. <span data-ttu-id="20583-151">V portálu Azure, na hello hello **Kintone** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="20583-151">In hello Azure portal, on hello **Kintone** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="20583-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="20583-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_samlbase.png)

3. <span data-ttu-id="20583-155">Na hello **Kintone domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="20583-155">On hello **Kintone Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_url.png)

    <span data-ttu-id="20583-157">a.</span><span class="sxs-lookup"><span data-stu-id="20583-157">a.</span></span> <span data-ttu-id="20583-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.kintone.com`</span><span class="sxs-lookup"><span data-stu-id="20583-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.kintone.com`</span></span>

    <span data-ttu-id="20583-159">b.</span><span class="sxs-lookup"><span data-stu-id="20583-159">b.</span></span> <span data-ttu-id="20583-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="20583-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > <span data-ttu-id="20583-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="20583-161">These values are not real.</span></span> <span data-ttu-id="20583-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="20583-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="20583-163">Obraťte se na [tým podpory Kintone klienta](https://www.kintone.com/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="20583-163">Contact [Kintone Client support team](https://www.kintone.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="20583-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="20583-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_certificate.png) 

5. <span data-ttu-id="20583-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20583-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="20583-168">Na hello **Kintone konfigurace** klikněte na tlačítko **konfigurace Kintone** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="20583-168">On hello **Kintone Configuration** section, click **Configure Kintone** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="20583-169">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="20583-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_configure.png) 

7. <span data-ttu-id="20583-171">V okně prohlížeče jiný web, přihlaste se k vaší **Kintone** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="20583-171">In a different web browser window, log into your **Kintone** company site as an administrator.</span></span>

8. <span data-ttu-id="20583-172">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="20583-172">Click **Settings**.</span></span>
   
    <span data-ttu-id="20583-173">![Nastavení](./media/active-directory-saas-kintone-tutorial/ic785879.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="20583-173">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

9. <span data-ttu-id="20583-174">Klikněte na tlačítko **uživatelů a správu systému**.</span><span class="sxs-lookup"><span data-stu-id="20583-174">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="20583-175">![Uživatelé a správu systému](./media/active-directory-saas-kintone-tutorial/ic785880.png "uživatelů a správu systému")</span><span class="sxs-lookup"><span data-stu-id="20583-175">![Users & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "Users & System Administration")</span></span>

10. <span data-ttu-id="20583-176">V části **systému správy \> zabezpečení** klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="20583-176">Under **System Administration \> Security** click **Login**.</span></span>
   
    <span data-ttu-id="20583-177">![Přihlášení](./media/active-directory-saas-kintone-tutorial/ic785881.png "přihlášení")</span><span class="sxs-lookup"><span data-stu-id="20583-177">![Login](./media/active-directory-saas-kintone-tutorial/ic785881.png "Login")</span></span>

11. <span data-ttu-id="20583-178">Klikněte na tlačítko **ověřování povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="20583-178">Click **Enable SAML authentication**.</span></span>
   
    <span data-ttu-id="20583-179">![Ověřování SAML](./media/active-directory-saas-kintone-tutorial/ic785882.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="20583-179">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785882.png "SAML Authentication")</span></span>

12. <span data-ttu-id="20583-180">V části ověřování SAML hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20583-180">In hello SAML Authentication section, perform hello following steps:</span></span>
    
    <span data-ttu-id="20583-181">![Ověřování SAML](./media/active-directory-saas-kintone-tutorial/ic785883.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="20583-181">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785883.png "SAML Authentication")</span></span>
    
    <span data-ttu-id="20583-182">a.</span><span class="sxs-lookup"><span data-stu-id="20583-182">a.</span></span> <span data-ttu-id="20583-183">V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="20583-183">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="20583-184">b.</span><span class="sxs-lookup"><span data-stu-id="20583-184">b.</span></span> <span data-ttu-id="20583-185">V hello **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="20583-185">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="20583-186">c.</span><span class="sxs-lookup"><span data-stu-id="20583-186">c.</span></span> <span data-ttu-id="20583-187">Klikněte na tlačítko **Procházet** tooupload stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="20583-187">Click **Browse** tooupload your downloaded certificate.</span></span>
    
    <span data-ttu-id="20583-188">d.</span><span class="sxs-lookup"><span data-stu-id="20583-188">d.</span></span> <span data-ttu-id="20583-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="20583-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="20583-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="20583-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="20583-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="20583-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="20583-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="20583-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="20583-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="20583-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="20583-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="20583-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="20583-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="20583-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="20583-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="20583-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="20583-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="20583-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="20583-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="20583-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="20583-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="20583-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kintone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="20583-205">a.</span><span class="sxs-lookup"><span data-stu-id="20583-205">a.</span></span> <span data-ttu-id="20583-206">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="20583-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="20583-207">b.</span><span class="sxs-lookup"><span data-stu-id="20583-207">b.</span></span> <span data-ttu-id="20583-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="20583-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="20583-209">c.</span><span class="sxs-lookup"><span data-stu-id="20583-209">c.</span></span> <span data-ttu-id="20583-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="20583-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="20583-211">d.</span><span class="sxs-lookup"><span data-stu-id="20583-211">d.</span></span> <span data-ttu-id="20583-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="20583-212">Click **Create**.</span></span>
 
### <a name="creating-a-kintone-test-user"></a><span data-ttu-id="20583-213">Vytvoření zkušebního uživatele Kintone</span><span class="sxs-lookup"><span data-stu-id="20583-213">Creating a Kintone test user</span></span>

<span data-ttu-id="20583-214">Uživatelé toolog tooenable Azure AD v tooKintone, se musí být zřízená do Kintone.</span><span class="sxs-lookup"><span data-stu-id="20583-214">tooenable Azure AD users toolog in tooKintone, they must be provisioned into Kintone.</span></span>  
<span data-ttu-id="20583-215">V případě hello Kintone zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="20583-215">In hello case of Kintone, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="20583-216">tooprovision uživatelský účet, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="20583-216">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="20583-217">Přihlaste se tooyour **Kintone** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="20583-217">Log in tooyour **Kintone** company site as an administrator.</span></span>

2. <span data-ttu-id="20583-218">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="20583-218">Click **Setting**.</span></span>
   
    <span data-ttu-id="20583-219">![Nastavení](./media/active-directory-saas-kintone-tutorial/ic785879.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="20583-219">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

3. <span data-ttu-id="20583-220">Klikněte na tlačítko **uživatelů a správu systému**.</span><span class="sxs-lookup"><span data-stu-id="20583-220">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="20583-221">![& Systém správy uživatelského](./media/active-directory-saas-kintone-tutorial/ic785880.png "uživatele & správu systému")</span><span class="sxs-lookup"><span data-stu-id="20583-221">![User & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "User & System Administration")</span></span>

4. <span data-ttu-id="20583-222">V části **Správa uživatele**, klikněte na tlačítko **oddělení a uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="20583-222">Under **User Administration**, click **Departments & Users**.</span></span>
   
    <span data-ttu-id="20583-223">![Oddělení a uživatelé](./media/active-directory-saas-kintone-tutorial/ic785888.png "oddělení a uživatelé")</span><span class="sxs-lookup"><span data-stu-id="20583-223">![Department & Users](./media/active-directory-saas-kintone-tutorial/ic785888.png "Department & Users")</span></span>

5. <span data-ttu-id="20583-224">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="20583-224">Click **New User**.</span></span>
   
    <span data-ttu-id="20583-225">![Noví uživatelé](./media/active-directory-saas-kintone-tutorial/ic785889.png "noví uživatelé")</span><span class="sxs-lookup"><span data-stu-id="20583-225">![New Users](./media/active-directory-saas-kintone-tutorial/ic785889.png "New Users")</span></span>

6. <span data-ttu-id="20583-226">V hello **nového uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="20583-226">In hello **New User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="20583-227">![Noví uživatelé](./media/active-directory-saas-kintone-tutorial/ic785890.png "noví uživatelé")</span><span class="sxs-lookup"><span data-stu-id="20583-227">![New Users](./media/active-directory-saas-kintone-tutorial/ic785890.png "New Users")</span></span>
   
    <span data-ttu-id="20583-228">a.</span><span class="sxs-lookup"><span data-stu-id="20583-228">a.</span></span> <span data-ttu-id="20583-229">Zadejte **zobrazovaný název**, **přihlašovací jméno**, **nové heslo**, **potvrzení hesla**, **e-mailová adresa**, a další podrobnosti o platný účet AAD, že chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="20583-229">Type a **Display Name**, **Login Name**, **New Password**, **Confirm Password**, **E-mail Address**, and other details of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
 
    <span data-ttu-id="20583-230">b.</span><span class="sxs-lookup"><span data-stu-id="20583-230">b.</span></span> <span data-ttu-id="20583-231">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="20583-231">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="20583-232">Můžete použít všechny ostatní Kintone uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Kintone tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="20583-232">You can use any other Kintone user account creation tools or APIs provided by Kintone tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="20583-233">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="20583-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="20583-234">V této části povolíte tak, že udělíte přístup tooKintone toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="20583-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKintone.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="20583-236">**tooassign Britta Simon tooKintone, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="20583-236">**tooassign Britta Simon tooKintone, perform hello following steps:**</span></span>

1. <span data-ttu-id="20583-237">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="20583-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="20583-239">V seznamu aplikace hello vyberte **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="20583-239">In hello applications list, select **Kintone**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_app.png) 

3. <span data-ttu-id="20583-241">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="20583-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="20583-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="20583-243">Click **Add** button.</span></span> <span data-ttu-id="20583-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20583-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="20583-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="20583-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="20583-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20583-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="20583-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="20583-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="20583-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="20583-249">Testing single sign-on</span></span>

<span data-ttu-id="20583-250">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="20583-250">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="20583-251">Když kliknete na dlaždici Kintone hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Kintone aplikace.</span><span class="sxs-lookup"><span data-stu-id="20583-251">When you click hello Kintone tile in hello Access Panel, you should get automatically signed-on tooyour Kintone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20583-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="20583-252">Additional resources</span></span>

* [<span data-ttu-id="20583-253">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="20583-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="20583-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="20583-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_203.png

