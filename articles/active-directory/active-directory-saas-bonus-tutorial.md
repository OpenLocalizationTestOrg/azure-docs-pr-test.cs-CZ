---
title: 'Kurz: Azure Active Directory integrace s Bonusly | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Bonusly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 60ad06c028463af81a7901ab321c4ae9346798ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a><span data-ttu-id="52640-103">Kurz: Azure Active Directory integrace s Bonusly</span><span class="sxs-lookup"><span data-stu-id="52640-103">Tutorial: Azure Active Directory integration with Bonusly</span></span>

<span data-ttu-id="52640-104">V tomto kurzu zjistíte, jak toointegrate Bonusly s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="52640-104">In this tutorial, you learn how toointegrate Bonusly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="52640-105">Integrace Bonusly s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="52640-105">Integrating Bonusly with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="52640-106">Můžete řídit ve službě Azure AD, který má přístup tooBonusly</span><span class="sxs-lookup"><span data-stu-id="52640-106">You can control in Azure AD who has access tooBonusly</span></span>
- <span data-ttu-id="52640-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBonusly (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="52640-107">You can enable your users tooautomatically get signed-on tooBonusly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="52640-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="52640-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="52640-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="52640-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52640-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="52640-110">Prerequisites</span></span>

<span data-ttu-id="52640-111">Integrace služby Azure AD s Bonusly tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="52640-111">tooconfigure Azure AD integration with Bonusly, you need hello following items:</span></span>

- <span data-ttu-id="52640-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="52640-112">An Azure AD subscription</span></span>
- <span data-ttu-id="52640-113">Bonusly jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="52640-113">A Bonusly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="52640-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="52640-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="52640-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="52640-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="52640-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="52640-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="52640-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="52640-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="52640-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="52640-118">Scenario description</span></span>
<span data-ttu-id="52640-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="52640-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="52640-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="52640-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="52640-121">Přidání Bonusly z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="52640-121">Adding Bonusly from hello gallery</span></span>
2. <span data-ttu-id="52640-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="52640-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bonusly-from-hello-gallery"></a><span data-ttu-id="52640-123">Přidání Bonusly z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="52640-123">Adding Bonusly from hello gallery</span></span>
<span data-ttu-id="52640-124">tooconfigure hello integrace Bonusly do Azure AD, je nutné tooadd Bonusly hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="52640-124">tooconfigure hello integration of Bonusly into Azure AD, you need tooadd Bonusly from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="52640-125">**tooadd Bonusly z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="52640-125">**tooadd Bonusly from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="52640-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="52640-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="52640-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="52640-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="52640-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="52640-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="52640-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="52640-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="52640-133">Hello vyhledávacího pole zadejte **Bonusly**, vyberte **Bonusly** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="52640-133">In hello search box, type **Bonusly**, select **Bonusly** from result panel then click **Add** button tooadd hello application.</span></span>

    ![V seznamu výsledků hello bonusly](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="52640-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="52640-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="52640-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Bonusly podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="52640-136">In this section, you configure and test Azure AD single sign-on with Bonusly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="52640-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Bonusly je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="52640-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bonusly is tooa user in Azure AD.</span></span> <span data-ttu-id="52640-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Bonusly musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="52640-138">In other words, a link relationship between an Azure AD user and hello related user in Bonusly needs toobe established.</span></span>

<span data-ttu-id="52640-139">V Bonusly, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="52640-139">In Bonusly, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="52640-140">tooconfigure a testu Azure AD jednotné přihlašování s Bonusly, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="52640-140">tooconfigure and test Azure AD single sign-on with Bonusly, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="52640-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="52640-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="52640-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52640-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="52640-143">**[Vytvoření Bonusly zkušebního uživatele](#create-a-bonusly-test-user)**  -toohave protějšek Britta Simon v Bonusly, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="52640-143">**[Create a Bonusly test user](#create-a-bonusly-test-user)** - toohave a counterpart of Britta Simon in Bonusly that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="52640-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="52640-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="52640-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="52640-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="52640-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="52640-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="52640-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Bonusly.</span><span class="sxs-lookup"><span data-stu-id="52640-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bonusly application.</span></span>

<span data-ttu-id="52640-148">**tooconfigure Azure AD jednotné přihlašování s Bonusly, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="52640-148">**tooconfigure Azure AD single sign-on with Bonusly, perform hello following steps:**</span></span>

1. <span data-ttu-id="52640-149">V portálu Azure, na hello hello **Bonusly** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="52640-149">In hello Azure portal, on hello **Bonusly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="52640-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="52640-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_samlbase.png)

3. <span data-ttu-id="52640-153">Na hello **Bonusly domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="52640-153">On hello **Bonusly Domain and URLs** section, perform hello following steps:</span></span>

    ![Bonusly domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_url.png)

    <span data-ttu-id="52640-155">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://Bonus.ly/saml/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="52640-155">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://Bonus.ly/saml/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="52640-156">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="52640-156">hello value is not real.</span></span> <span data-ttu-id="52640-157">Aktualizace hello hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="52640-157">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="52640-158">Obraťte se na [tým podpory Bonusly](https://Bonusly/contact) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="52640-158">Contact [Bonusly support team](https://Bonusly/contact) tooget hello value.</span></span>
 
4. <span data-ttu-id="52640-159">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnotu z hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="52640-159">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_certificate.png) 

5. <span data-ttu-id="52640-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="52640-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-bonus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="52640-163">Na hello **Bonusly konfigurace** klikněte na tlačítko **konfigurace Bonusly** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="52640-163">On hello **Bonusly Configuration** section, click **Configure Bonusly** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="52640-164">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="52640-164">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Bonusly konfigurace](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_configure.png) 

7. <span data-ttu-id="52640-166">V okně jiný prohlížeč, přihlaste tooyour **Bonusly** klienta.</span><span class="sxs-lookup"><span data-stu-id="52640-166">In a different browser window, log in tooyour **Bonusly** tenant.</span></span>

8. <span data-ttu-id="52640-167">V panelu nástrojů hello hello nahoře, klikněte na **nastavení**a potom vyberte **aplikace a integrace v rámci**.</span><span class="sxs-lookup"><span data-stu-id="52640-167">In hello toolbar on hello top, click **Settings**, and then select **Integrations and apps**.</span></span>
   
    <span data-ttu-id="52640-168">![Bonusly sociálních části](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="52640-168">![Bonusly Social Section](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span></span>
9. <span data-ttu-id="52640-169">V části **jednotné přihlašování**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="52640-169">Under **Single Sign-On**, select **SAML**.</span></span>

10. <span data-ttu-id="52640-170">Na hello **SAML** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="52640-170">On hello **SAML** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="52640-171">![Bonusly Saml dialogové okno stránky](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="52640-171">![Bonusly Saml Dialog page](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span></span>
   
    <span data-ttu-id="52640-172">a.</span><span class="sxs-lookup"><span data-stu-id="52640-172">a.</span></span> <span data-ttu-id="52640-173">V hello **IdP SSO cílová adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="52640-173">In hello **IdP SSO target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="52640-174">b.</span><span class="sxs-lookup"><span data-stu-id="52640-174">b.</span></span> <span data-ttu-id="52640-175">V hello **IdP vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="52640-175">In hello **IdP Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="52640-176">c.</span><span class="sxs-lookup"><span data-stu-id="52640-176">c.</span></span> <span data-ttu-id="52640-177">V hello **IdP přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="52640-177">In hello **IdP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="52640-178">d.</span><span class="sxs-lookup"><span data-stu-id="52640-178">d.</span></span> <span data-ttu-id="52640-179">Vložení **kryptografický otisk** hodnota zkopírována z portálu Azure do hello **otisk prstu Cert** textové pole.</span><span class="sxs-lookup"><span data-stu-id="52640-179">Paste the **Thumbprint** value copied from Azure portal into hello **Cert Fingerprint** textbox.</span></span>
   
11. <span data-ttu-id="52640-180">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52640-180">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="52640-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="52640-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="52640-182">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="52640-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="52640-183">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="52640-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="52640-184">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="52640-184">Create an Azure AD test user</span></span>
<span data-ttu-id="52640-185">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="52640-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="52640-187">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="52640-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="52640-188">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="52640-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-bonus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="52640-190">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="52640-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-bonus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="52640-192">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="52640-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-bonus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="52640-194">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="52640-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-bonus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="52640-196">a.</span><span class="sxs-lookup"><span data-stu-id="52640-196">a.</span></span> <span data-ttu-id="52640-197">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="52640-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="52640-198">b.</span><span class="sxs-lookup"><span data-stu-id="52640-198">b.</span></span> <span data-ttu-id="52640-199">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="52640-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="52640-200">c.</span><span class="sxs-lookup"><span data-stu-id="52640-200">c.</span></span> <span data-ttu-id="52640-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="52640-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="52640-202">d.</span><span class="sxs-lookup"><span data-stu-id="52640-202">d.</span></span> <span data-ttu-id="52640-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="52640-203">Click **Create**.</span></span>
 
### <a name="create-a-bonusly-test-user"></a><span data-ttu-id="52640-204">Vytvoření Bonusly zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="52640-204">Create a Bonusly test user</span></span>

<span data-ttu-id="52640-205">V pořadí tooenable Azure AD Uživatelé toolog v tooBonusly musí být zřízená do Bonusly.</span><span class="sxs-lookup"><span data-stu-id="52640-205">In order tooenable Azure AD users toolog in tooBonusly, they must be provisioned into Bonusly.</span></span> <span data-ttu-id="52640-206">V případě hello Bonusly zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="52640-206">In hello case of Bonusly, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="52640-207">Můžete použít jakékoli jiné nástroje vytvoření Bonusly uživatelského účtu nebo rozhraní API poskytované Bonusly tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="52640-207">You can use any other Bonusly user account creation tools or APIs provided by Bonusly tooprovision AAD user accounts.</span></span>
>  

<span data-ttu-id="52640-208">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="52640-208">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="52640-209">V okně webového prohlížeče Přihlaste se tooyour Bonusly klienta.</span><span class="sxs-lookup"><span data-stu-id="52640-209">In a web browser window, log in tooyour Bonusly tenant.</span></span>

2. <span data-ttu-id="52640-210">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="52640-210">Click **Settings**.</span></span>
 
    <span data-ttu-id="52640-211">![Nastavení](./media/active-directory-saas-bonus-tutorial/ic781041.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="52640-211">![Settings](./media/active-directory-saas-bonus-tutorial/ic781041.png "Settings")</span></span>

3. <span data-ttu-id="52640-212">Klikněte na tlačítko hello **uživatelů a bonusy** kartě.</span><span class="sxs-lookup"><span data-stu-id="52640-212">Click hello **Users and bonuses** tab.</span></span>
   
    <span data-ttu-id="52640-213">![Uživatelé a bonusy](./media/active-directory-saas-bonus-tutorial/ic781042.png "uživatelů a bonusy")</span><span class="sxs-lookup"><span data-stu-id="52640-213">![Users and bonuses](./media/active-directory-saas-bonus-tutorial/ic781042.png "Users and bonuses")</span></span>

4. <span data-ttu-id="52640-214">Klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="52640-214">Click **Manage Users**.</span></span>
   
    <span data-ttu-id="52640-215">![Správa uživatelů](./media/active-directory-saas-bonus-tutorial/ic781043.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="52640-215">![Manage Users](./media/active-directory-saas-bonus-tutorial/ic781043.png "Manage Users")</span></span>

5. <span data-ttu-id="52640-216">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="52640-216">Click **Add User**.</span></span>
   
    <span data-ttu-id="52640-217">![Přidat uživatele](./media/active-directory-saas-bonus-tutorial/ic781044.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="52640-217">![Add User](./media/active-directory-saas-bonus-tutorial/ic781044.png "Add User")</span></span>

6. <span data-ttu-id="52640-218">Na hello **přidat uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="52640-218">On hello **Add User** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="52640-219">![Přidat uživatele](./media/active-directory-saas-bonus-tutorial/ic781045.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="52640-219">![Add User](./media/active-directory-saas-bonus-tutorial/ic781045.png "Add User")</span></span>  

    <span data-ttu-id="52640-220">a.</span><span class="sxs-lookup"><span data-stu-id="52640-220">a.</span></span> <span data-ttu-id="52640-221">V hello **křestní jméno** textovému poli, zadejte hello křestní jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="52640-221">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="52640-222">b.</span><span class="sxs-lookup"><span data-stu-id="52640-222">b.</span></span> <span data-ttu-id="52640-223">V hello **příjmení** textovému poli, zadejte příjmení uživatele jako hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="52640-223">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="52640-224">c.</span><span class="sxs-lookup"><span data-stu-id="52640-224">c.</span></span> <span data-ttu-id="52640-225">V hello **e-mailu** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="52640-225">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="52640-226">d.</span><span class="sxs-lookup"><span data-stu-id="52640-226">d.</span></span> <span data-ttu-id="52640-227">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="52640-227">Click **Save**.</span></span>
   
     >[!NOTE]
     ><span data-ttu-id="52640-228">Držitel účtu Azure AD Hello obdrží e-mail, který obsahuje účet odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="52640-228">hello Azure AD account holder receives an email that includes a link tooconfirm hello account before it becomes active.</span></span>
     >  

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="52640-229">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="52640-229">Assign hello Azure AD test user</span></span>

<span data-ttu-id="52640-230">V této části povolíte tak, že udělíte přístup tooBonusly toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="52640-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBonusly.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="52640-232">**tooassign Britta Simon tooBonusly, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="52640-232">**tooassign Britta Simon tooBonusly, perform hello following steps:**</span></span>

1. <span data-ttu-id="52640-233">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="52640-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="52640-235">V seznamu aplikace hello vyberte **Bonusly**.</span><span class="sxs-lookup"><span data-stu-id="52640-235">In hello applications list, select **Bonusly**.</span></span>

    ![Hello Bonusly odkaz v seznamu aplikace hello](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_app.png) 

3. <span data-ttu-id="52640-237">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="52640-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="52640-239">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="52640-239">Click **Add** button.</span></span> <span data-ttu-id="52640-240">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52640-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="52640-242">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="52640-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="52640-243">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52640-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="52640-244">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="52640-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="52640-245">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="52640-245">Test single sign-on</span></span>

<span data-ttu-id="52640-246">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="52640-246">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="52640-247">Po kliknutí na tlačítko hello Bonusly dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Bonusly aplikace.</span><span class="sxs-lookup"><span data-stu-id="52640-247">When you click hello Bonusly tile in hello Access Panel, you should get automatically signed-on tooyour Bonusly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52640-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="52640-248">Additional resources</span></span>

* [<span data-ttu-id="52640-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="52640-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="52640-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="52640-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_203.png

