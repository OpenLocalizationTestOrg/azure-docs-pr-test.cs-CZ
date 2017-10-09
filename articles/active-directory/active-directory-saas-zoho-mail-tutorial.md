---
title: 'Kurz: Azure Active Directory integrace s Zoho | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Zoho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 5d1c196d3b97babab196f509ea68e7d61523a7e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a><span data-ttu-id="599bf-103">Kurz: Azure Active Directory integrace s Zoho</span><span class="sxs-lookup"><span data-stu-id="599bf-103">Tutorial: Azure Active Directory integration with Zoho</span></span>

<span data-ttu-id="599bf-104">V tomto kurzu zjistíte, jak toointegrate Zoho s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="599bf-104">In this tutorial, you learn how toointegrate Zoho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="599bf-105">Integrace Zoho s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="599bf-105">Integrating Zoho with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="599bf-106">Můžete ovládat ve službě Azure AD, který má přístup tooZoho.</span><span class="sxs-lookup"><span data-stu-id="599bf-106">You can control in Azure AD who has access tooZoho.</span></span>
- <span data-ttu-id="599bf-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZoho (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="599bf-107">You can enable your users tooautomatically get signed-on tooZoho (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="599bf-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="599bf-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="599bf-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="599bf-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="599bf-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="599bf-110">Prerequisites</span></span>

<span data-ttu-id="599bf-111">Integrace služby Azure AD s Zoho tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="599bf-111">tooconfigure Azure AD integration with Zoho, you need hello following items:</span></span>

- <span data-ttu-id="599bf-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="599bf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="599bf-113">Zoho jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="599bf-113">A Zoho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="599bf-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="599bf-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="599bf-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="599bf-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="599bf-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="599bf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="599bf-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="599bf-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="599bf-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="599bf-118">Scenario description</span></span>
<span data-ttu-id="599bf-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="599bf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="599bf-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="599bf-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="599bf-121">Přidání Zoho z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="599bf-121">Adding Zoho from hello gallery</span></span>
2. <span data-ttu-id="599bf-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="599bf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoho-from-hello-gallery"></a><span data-ttu-id="599bf-123">Přidání Zoho z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="599bf-123">Adding Zoho from hello gallery</span></span>
<span data-ttu-id="599bf-124">tooconfigure hello integrace Zoho do Azure AD, je nutné tooadd Zoho hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="599bf-124">tooconfigure hello integration of Zoho into Azure AD, you need tooadd Zoho from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="599bf-125">**tooadd Zoho z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="599bf-125">**tooadd Zoho from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="599bf-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="599bf-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="599bf-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="599bf-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="599bf-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="599bf-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="599bf-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="599bf-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="599bf-133">Hello vyhledávacího pole zadejte **Zoho**, vyberte **Zoho** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="599bf-133">In hello search box, type **Zoho**, select **Zoho** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Zoho v seznamu výsledků hello](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="599bf-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="599bf-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="599bf-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zoho podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="599bf-136">In this section, you configure and test Azure AD single sign-on with Zoho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="599bf-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Zoho je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="599bf-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zoho is tooa user in Azure AD.</span></span> <span data-ttu-id="599bf-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Zoho musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="599bf-138">In other words, a link relationship between an Azure AD user and hello related user in Zoho needs toobe established.</span></span>

<span data-ttu-id="599bf-139">V Zoho, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="599bf-139">In Zoho, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="599bf-140">tooconfigure a testu Azure AD jednotné přihlašování s Zoho, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="599bf-140">tooconfigure and test Azure AD single sign-on with Zoho, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="599bf-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="599bf-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="599bf-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="599bf-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="599bf-143">**[Vytvoření zkušebního uživatele Zoho](#create-a-zoho-test-user)**  -toohave protějšek Britta Simon v Zoho, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="599bf-143">**[Create a Zoho test user](#create-a-zoho-test-user)** - toohave a counterpart of Britta Simon in Zoho that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="599bf-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="599bf-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="599bf-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="599bf-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="599bf-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="599bf-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="599bf-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Zoho.</span><span class="sxs-lookup"><span data-stu-id="599bf-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zoho application.</span></span>

<span data-ttu-id="599bf-148">**tooconfigure Azure AD jednotné přihlašování s Zoho, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="599bf-148">**tooconfigure Azure AD single sign-on with Zoho, perform hello following steps:**</span></span>

1. <span data-ttu-id="599bf-149">V portálu Azure, na hello hello **Zoho** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="599bf-149">In hello Azure portal, on hello **Zoho** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="599bf-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="599bf-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. <span data-ttu-id="599bf-153">Na hello **Zoho domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="599bf-153">On hello **Zoho Domain and URLs** section, perform hello following steps:</span></span>

    ![Zoho domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    <span data-ttu-id="599bf-155">a.</span><span class="sxs-lookup"><span data-stu-id="599bf-155">a.</span></span> <span data-ttu-id="599bf-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.zohomail.com`</span><span class="sxs-lookup"><span data-stu-id="599bf-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.zohomail.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="599bf-157">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="599bf-157">This value is not real.</span></span> <span data-ttu-id="599bf-158">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="599bf-158">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="599bf-159">Obraťte se na [tým podpory Zoho klienta](https://www.zoho.com/mail/contact.html) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="599bf-159">Contact [Zoho Client support team](https://www.zoho.com/mail/contact.html) tooget this value.</span></span> 
 
4. <span data-ttu-id="599bf-160">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="599bf-160">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. <span data-ttu-id="599bf-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="599bf-162">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="599bf-164">Na hello **Zoho konfigurace** klikněte na tlačítko **konfigurace Zoho** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="599bf-164">On hello **Zoho Configuration** section, click **Configure Zoho** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="599bf-165">Kopírování hello **Sign-Out adresu URL, adresy URL pro změnu hesla a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="599bf-165">Copy hello **Sign-Out URL, Change Password URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Zoho](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. <span data-ttu-id="599bf-167">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zoho e-mailu.</span><span class="sxs-lookup"><span data-stu-id="599bf-167">In a different web browser window, log into your Zoho Mail company site as an administrator.</span></span>

8. <span data-ttu-id="599bf-168">Přejděte toohello **ovládací panely**.</span><span class="sxs-lookup"><span data-stu-id="599bf-168">Go toohello **Control panel**.</span></span>
   
    <span data-ttu-id="599bf-169">![Ovládací panely](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "ovládací panely")</span><span class="sxs-lookup"><span data-stu-id="599bf-169">![Control Panel](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Control Panel")</span></span>

9. <span data-ttu-id="599bf-170">Klikněte na tlačítko hello **ověřování SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="599bf-170">Click hello **SAML Authentication** tab.</span></span>
   
    <span data-ttu-id="599bf-171">![Ověřování SAML](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="599bf-171">![SAML Authentication](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML Authentication")</span></span>

10. <span data-ttu-id="599bf-172">V hello **podrobnosti ověřování SAML** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="599bf-172">In hello **SAML Authentication Details** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="599bf-173">![Podrobnosti o ověřování SAML](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "podrobnosti ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="599bf-173">![SAML Authentication Details](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML Authentication Details")</span></span>
   
    <span data-ttu-id="599bf-174">a.</span><span class="sxs-lookup"><span data-stu-id="599bf-174">a.</span></span> <span data-ttu-id="599bf-175">V hello **přihlašovací adresa URL** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="599bf-175">In hello **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="599bf-176">b.</span><span class="sxs-lookup"><span data-stu-id="599bf-176">b.</span></span> <span data-ttu-id="599bf-177">V hello **adresy URL odhlašovací** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="599bf-177">In hello **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="599bf-178">c.</span><span class="sxs-lookup"><span data-stu-id="599bf-178">c.</span></span> <span data-ttu-id="599bf-179">V hello **heslo změnit adresu URL** textovému poli, vložte **heslo změnit adresu URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="599bf-179">In hello **Change Password URL** textbox, paste **Change Password URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="599bf-180">d.</span><span class="sxs-lookup"><span data-stu-id="599bf-180">d.</span></span> <span data-ttu-id="599bf-181">Otevření kódovaného certifikátu kódování base-64 stáhli z portálu Azure v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **PublicKey** textové pole.</span><span class="sxs-lookup"><span data-stu-id="599bf-181">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **PublicKey** textbox.</span></span>
   
    <span data-ttu-id="599bf-182">e.</span><span class="sxs-lookup"><span data-stu-id="599bf-182">e.</span></span> <span data-ttu-id="599bf-183">Jako **algoritmus**, vyberte **RSA**.</span><span class="sxs-lookup"><span data-stu-id="599bf-183">As **Algorithm**, select **RSA**.</span></span>
   
    <span data-ttu-id="599bf-184">f.</span><span class="sxs-lookup"><span data-stu-id="599bf-184">f.</span></span> <span data-ttu-id="599bf-185">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="599bf-185">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="599bf-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="599bf-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="599bf-187">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="599bf-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="599bf-188">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="599bf-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="599bf-189">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="599bf-189">Create an Azure AD test user</span></span>

<span data-ttu-id="599bf-190">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="599bf-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="599bf-192">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="599bf-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="599bf-193">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="599bf-193">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="599bf-195">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="599bf-195">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="599bf-197">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="599bf-197">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="599bf-199">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="599bf-199">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    <span data-ttu-id="599bf-201">a.</span><span class="sxs-lookup"><span data-stu-id="599bf-201">a.</span></span> <span data-ttu-id="599bf-202">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="599bf-202">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="599bf-203">b.</span><span class="sxs-lookup"><span data-stu-id="599bf-203">b.</span></span> <span data-ttu-id="599bf-204">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="599bf-204">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="599bf-205">c.</span><span class="sxs-lookup"><span data-stu-id="599bf-205">c.</span></span> <span data-ttu-id="599bf-206">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="599bf-206">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="599bf-207">d.</span><span class="sxs-lookup"><span data-stu-id="599bf-207">d.</span></span> <span data-ttu-id="599bf-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="599bf-208">Click **Create**.</span></span>
 
### <a name="create-a-zoho-test-user"></a><span data-ttu-id="599bf-209">Vytvoření zkušebního uživatele Zoho</span><span class="sxs-lookup"><span data-stu-id="599bf-209">Create a Zoho test user</span></span>

<span data-ttu-id="599bf-210">V pořadí tooenable Azure AD Uživatelé toolog do Zoho e-mailu musí být zřízená do Zoho e-mailu.</span><span class="sxs-lookup"><span data-stu-id="599bf-210">In order tooenable Azure AD users toolog into Zoho Mail, they must be provisioned into Zoho Mail.</span></span> <span data-ttu-id="599bf-211">V případě hello Zoho pošty zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="599bf-211">In hello case of Zoho Mail, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="599bf-212">Můžete použít jakékoli jiné e-mailu Zoho uživatele účtu vytvoření nástroje nebo rozhraní API poskytované e-mailu Zoho tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="599bf-212">You can use any other Zoho Mail user account creation tools or APIs provided by Zoho Mail tooprovision AAD user accounts.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="599bf-213">tooprovision uživatelský účet, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="599bf-213">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="599bf-214">Přihlaste se tooyour **Zoho e-mailu** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="599bf-214">Log in tooyour **Zoho Mail** company site as an administrator.</span></span>

2. <span data-ttu-id="599bf-215">Přejděte příliš**ovládací panely \> e-mailu a dokumentů**.</span><span class="sxs-lookup"><span data-stu-id="599bf-215">Go too**Control Panel \> Mail & Docs**.</span></span>

3. <span data-ttu-id="599bf-216">Přejděte příliš**podrobné informace o uživateli \> přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="599bf-216">Go too**User Details \> Add User**.</span></span>
   
    <span data-ttu-id="599bf-217">![Přidat uživatele](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="599bf-217">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "Add User")</span></span>

4. <span data-ttu-id="599bf-218">Na hello **přidat uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="599bf-218">On hello **Add users** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="599bf-219">![Přidat uživatele](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="599bf-219">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "Add User")</span></span>
   
    <span data-ttu-id="599bf-220">a.</span><span class="sxs-lookup"><span data-stu-id="599bf-220">a.</span></span> <span data-ttu-id="599bf-221">V hello **křestní jméno** textovému poli, typ hello křestní jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="599bf-221">In hello **First Name** textbox, type hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="599bf-222">b.</span><span class="sxs-lookup"><span data-stu-id="599bf-222">b.</span></span> <span data-ttu-id="599bf-223">V hello **příjmení** textovému poli, typ hello příjmení uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="599bf-223">In hello **Last Name** textbox, type hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="599bf-224">c.</span><span class="sxs-lookup"><span data-stu-id="599bf-224">c.</span></span> <span data-ttu-id="599bf-225">V hello **ID e-mailu** textovému poli, typ hello e-mailu id uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="599bf-225">In hello **Email ID** textbox, type hello email id of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="599bf-226">d.</span><span class="sxs-lookup"><span data-stu-id="599bf-226">d.</span></span> <span data-ttu-id="599bf-227">V hello **heslo** textovému poli, zadejte heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="599bf-227">In hello **Password** textbox, enter password of user.</span></span>
   
    <span data-ttu-id="599bf-228">e.</span><span class="sxs-lookup"><span data-stu-id="599bf-228">e.</span></span> <span data-ttu-id="599bf-229">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="599bf-229">Click **OK**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="599bf-230">Držitel účtu Azure Active Directory Hello obdrží e-mail s účtem odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="599bf-230">hello Azure Active Directory account holder will receive an email with a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="599bf-231">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="599bf-231">Assign hello Azure AD test user</span></span>

<span data-ttu-id="599bf-232">V této části povolíte tak, že udělíte přístup tooZoho toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="599bf-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZoho.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="599bf-234">**tooassign Britta Simon tooZoho, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="599bf-234">**tooassign Britta Simon tooZoho, perform hello following steps:**</span></span>

1. <span data-ttu-id="599bf-235">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="599bf-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="599bf-237">V seznamu aplikace hello vyberte **Zoho**.</span><span class="sxs-lookup"><span data-stu-id="599bf-237">In hello applications list, select **Zoho**.</span></span>

    ![v seznamu aplikace hello Hello Zoho odkaz](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. <span data-ttu-id="599bf-239">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="599bf-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="599bf-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="599bf-241">Click **Add** button.</span></span> <span data-ttu-id="599bf-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="599bf-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="599bf-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="599bf-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="599bf-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="599bf-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="599bf-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="599bf-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="599bf-247">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="599bf-247">Test single sign-on</span></span>

<span data-ttu-id="599bf-248">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="599bf-248">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="599bf-249">Když kliknete na dlaždici Zoho hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zoho aplikace.</span><span class="sxs-lookup"><span data-stu-id="599bf-249">When you click hello Zoho tile in hello Access Panel, you should get automatically signed-on tooyour Zoho application.</span></span>
<span data-ttu-id="599bf-250">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="599bf-250">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="599bf-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="599bf-251">Additional resources</span></span>

* [<span data-ttu-id="599bf-252">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="599bf-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="599bf-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="599bf-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

