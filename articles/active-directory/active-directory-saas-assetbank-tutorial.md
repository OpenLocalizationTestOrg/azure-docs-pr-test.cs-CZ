---
title: 'Kurz: Azure Active Directory integrace s Asset Bank | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Asset Bank."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3006ad6e-8831-41cd-94aa-7e7ae770ce7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 32cb355fbe16557eca69dbad1d3e6fbe19b53517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asset-bank"></a><span data-ttu-id="a897e-103">Kurz: Azure Active Directory integrace s Asset Bank</span><span class="sxs-lookup"><span data-stu-id="a897e-103">Tutorial: Azure Active Directory integration with Asset Bank</span></span>

<span data-ttu-id="a897e-104">V tomto kurzu zjistíte, jak toointegrate Asset bankovní službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a897e-104">In this tutorial, you learn how toointegrate Asset Bank with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a897e-105">Integrace Bank Asset s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a897e-105">Integrating Asset Bank with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a897e-106">Můžete řídit ve službě Azure AD, který má přístup tooAsset Bank</span><span class="sxs-lookup"><span data-stu-id="a897e-106">You can control in Azure AD who has access tooAsset Bank</span></span>
- <span data-ttu-id="a897e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAsset Bank (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a897e-107">You can enable your users tooautomatically get signed-on tooAsset Bank (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a897e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a897e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a897e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a897e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a897e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a897e-110">Prerequisites</span></span>

<span data-ttu-id="a897e-111">tooconfigure integrace Azure AD s Asset Bank, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a897e-111">tooconfigure Azure AD integration with Asset Bank, you need hello following items:</span></span>

- <span data-ttu-id="a897e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a897e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a897e-113">Asset Bank jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a897e-113">An Asset Bank single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a897e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a897e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a897e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a897e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a897e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a897e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a897e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a897e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a897e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a897e-118">Scenario description</span></span>
<span data-ttu-id="a897e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a897e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a897e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a897e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a897e-121">Přidání Asset Bank z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a897e-121">Adding Asset Bank from hello gallery</span></span>
2. <span data-ttu-id="a897e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a897e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asset-bank-from-hello-gallery"></a><span data-ttu-id="a897e-123">Přidání Asset Bank z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a897e-123">Adding Asset Bank from hello gallery</span></span>
<span data-ttu-id="a897e-124">tooconfigure hello integrace Asset Bank do Azure AD, je nutné tooadd Asset Bank hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a897e-124">tooconfigure hello integration of Asset Bank into Azure AD, you need tooadd Asset Bank from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a897e-125">**tooadd Asset Bank z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a897e-125">**tooadd Asset Bank from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a897e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a897e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a897e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a897e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a897e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a897e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a897e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a897e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a897e-133">Hello vyhledávacího pole zadejte **Asset Bank**.</span><span class="sxs-lookup"><span data-stu-id="a897e-133">In hello search box, type **Asset Bank**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_search.png)

5. <span data-ttu-id="a897e-135">Na panelu výsledků hello vyberte **Asset Bank**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a897e-135">In hello results panel, select **Asset Bank**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a897e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a897e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a897e-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Asset Bank podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="a897e-138">In this section, you configure and test Azure AD single sign-on with Asset Bank based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a897e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v bance Asset je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a897e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Asset Bank is tooa user in Azure AD.</span></span> <span data-ttu-id="a897e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v bance Asset musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a897e-140">In other words, a link relationship between an Azure AD user and hello related user in Asset Bank needs toobe established.</span></span>

<span data-ttu-id="a897e-141">V bance Asset přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="a897e-141">In Asset Bank, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a897e-142">tooconfigure a testu Azure AD jednotné přihlašování s Asset Bank, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a897e-142">tooconfigure and test Azure AD single sign-on with Asset Bank, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a897e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a897e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a897e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a897e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a897e-145">**[Vytváření testovacího uživatele Asset Bank](#creating-an-asset-bank-test-user)**  -toohave protějšek Britta Simon v bance Asset, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="a897e-145">**[Creating an Asset Bank test user](#creating-an-asset-bank-test-user)** - toohave a counterpart of Britta Simon in Asset Bank that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a897e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a897e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a897e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a897e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a897e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a897e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a897e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Asset Bank.</span><span class="sxs-lookup"><span data-stu-id="a897e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Asset Bank application.</span></span>

<span data-ttu-id="a897e-150">**tooconfigure Azure AD jednotné přihlašování s Asset Bank, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a897e-150">**tooconfigure Azure AD single sign-on with Asset Bank, perform hello following steps:**</span></span>

1. <span data-ttu-id="a897e-151">V portálu Azure, na hello hello **Asset Bank** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a897e-151">In hello Azure portal, on hello **Asset Bank** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a897e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a897e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_samlbase.png)

3. <span data-ttu-id="a897e-155">Na hello **Asset Bank domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a897e-155">On hello **Asset Bank Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_url.png)

    <span data-ttu-id="a897e-157">a.</span><span class="sxs-lookup"><span data-stu-id="a897e-157">a.</span></span> <span data-ttu-id="a897e-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.assetbank-server.com`</span><span class="sxs-lookup"><span data-stu-id="a897e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.assetbank-server.com`</span></span>

    <span data-ttu-id="a897e-159">b.</span><span class="sxs-lookup"><span data-stu-id="a897e-159">b.</span></span> <span data-ttu-id="a897e-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.assetbank-server.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="a897e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.assetbank-server.com/shibboleth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a897e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a897e-161">These values are not real.</span></span> <span data-ttu-id="a897e-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a897e-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a897e-163">Obraťte se na [tým podpory Asset Bank klienta](mailto:support@assetbank.co.uk) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a897e-163">Contact [Asset Bank Client support team](mailto:support@assetbank.co.uk) tooget these values.</span></span> 
 
4. <span data-ttu-id="a897e-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a897e-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_certificate.png) 

5. <span data-ttu-id="a897e-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a897e-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a897e-168">tooconfigure jednotného přihlašování na **Asset Bank** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Asset Bank](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="a897e-168">tooconfigure single sign-on on **Asset Bank** side, you need toosend hello downloaded **Metadata XML** too[Asset Bank support team](mailto:support@assetbank.co.uk).</span></span> 


> [!TIP]
> <span data-ttu-id="a897e-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="a897e-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a897e-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="a897e-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a897e-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a897e-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a897e-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a897e-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="a897e-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="a897e-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a897e-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a897e-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a897e-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a897e-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a897e-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a897e-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a897e-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="a897e-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a897e-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a897e-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a897e-184">a.</span><span class="sxs-lookup"><span data-stu-id="a897e-184">a.</span></span> <span data-ttu-id="a897e-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a897e-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a897e-186">b.</span><span class="sxs-lookup"><span data-stu-id="a897e-186">b.</span></span> <span data-ttu-id="a897e-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a897e-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a897e-188">c.</span><span class="sxs-lookup"><span data-stu-id="a897e-188">c.</span></span> <span data-ttu-id="a897e-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a897e-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a897e-190">d.</span><span class="sxs-lookup"><span data-stu-id="a897e-190">d.</span></span> <span data-ttu-id="a897e-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a897e-191">Click **Create**.</span></span>
 
### <a name="creating-an-asset-bank-test-user"></a><span data-ttu-id="a897e-192">Vytvoření Asset Bank testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a897e-192">Creating an Asset Bank test user</span></span>

<span data-ttu-id="a897e-193">Hello cílem této části je toocreate uživatele volat Britta Simon v bance Asset.</span><span class="sxs-lookup"><span data-stu-id="a897e-193">hello objective of this section is toocreate a user called Britta Simon in Asset Bank.</span></span> <span data-ttu-id="a897e-194">Asset Bank podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="a897e-194">Asset Bank supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="a897e-195">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="a897e-195">There is no action item for you in this section.</span></span> <span data-ttu-id="a897e-196">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Asset Bank.</span><span class="sxs-lookup"><span data-stu-id="a897e-196">A new user is created during an attempt tooaccess Asset Bank if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="a897e-197">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Asset Bank](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="a897e-197">If you need toocreate a user manually, you need toocontact hello [Asset Bank support team](mailto:support@assetbank.co.uk).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a897e-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a897e-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a897e-199">V této části povolíte tak, že udělíte přístup tooAsset Bank Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a897e-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAsset Bank.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a897e-201">**tooassign Britta Simon tooAsset Bank, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a897e-201">**tooassign Britta Simon tooAsset Bank, perform hello following steps:**</span></span>

1. <span data-ttu-id="a897e-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a897e-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a897e-204">V seznamu aplikace hello vyberte **Asset Bank**.</span><span class="sxs-lookup"><span data-stu-id="a897e-204">In hello applications list, select **Asset Bank**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_app.png) 

3. <span data-ttu-id="a897e-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a897e-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a897e-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a897e-208">Click **Add** button.</span></span> <span data-ttu-id="a897e-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a897e-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a897e-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="a897e-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a897e-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a897e-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a897e-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a897e-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a897e-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a897e-214">Testing single sign-on</span></span>

<span data-ttu-id="a897e-215">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a897e-215">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a897e-216">Když kliknete na dlaždici Asset Bank hello v hello přístupového panelu, měli byste obdržet aplikaci pro bankovní Asset automaticky přihlášeného tooyour.</span><span class="sxs-lookup"><span data-stu-id="a897e-216">When you click hello Asset Bank tile in hello Access Panel, you should get automatically signed-on tooyour Asset Bank application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a897e-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a897e-217">Additional resources</span></span>

* [<span data-ttu-id="a897e-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a897e-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a897e-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a897e-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_203.png

