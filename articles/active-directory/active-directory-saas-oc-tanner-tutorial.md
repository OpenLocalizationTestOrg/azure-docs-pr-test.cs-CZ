---
title: "Kurz: Azure Active Directory integrace s O.C. Nováková - AppreciateHub | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a O.C. Nováková - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 45052cf56e35746d7df5910162e40e3bbcad1aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a><span data-ttu-id="7c07d-105">Kurz: Azure Active Directory integrace s O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-105">Tutorial: Azure Active Directory integration with O.C.</span></span> <span data-ttu-id="7c07d-106">Nováková - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="7c07d-106">Tanner - AppreciateHub</span></span>

<span data-ttu-id="7c07d-107">V tomto kurzu zjistíte, jak toointegrate O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-107">In this tutorial, you learn how toointegrate O.C.</span></span> <span data-ttu-id="7c07d-108">Nováková - AppreciateHub s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7c07d-108">Tanner - AppreciateHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7c07d-109">Integrace O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-109">Integrating O.C.</span></span> <span data-ttu-id="7c07d-110">Nováková - AppreciateHub s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7c07d-110">Tanner - AppreciateHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7c07d-111">Můžete ovládat ve službě Azure AD, který má přístup tooO.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-111">You can control in Azure AD who has access tooO.C.</span></span> <span data-ttu-id="7c07d-112">Nováková - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="7c07d-112">Tanner - AppreciateHub</span></span>
- <span data-ttu-id="7c07d-113">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooO.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-113">You can enable your users tooautomatically get signed-on tooO.C.</span></span> <span data-ttu-id="7c07d-114">Nováková - AppreciateHub (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c07d-114">Tanner - AppreciateHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7c07d-115">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7c07d-115">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7c07d-116">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7c07d-116">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c07d-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7c07d-117">Prerequisites</span></span>

<span data-ttu-id="7c07d-118">tooconfigure integrace Azure AD s O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-118">tooconfigure Azure AD integration with O.C.</span></span> <span data-ttu-id="7c07d-119">Nováková - AppreciateHub, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7c07d-119">Tanner - AppreciateHub, you need hello following items:</span></span>

- <span data-ttu-id="7c07d-120">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c07d-120">An Azure AD subscription</span></span>
- <span data-ttu-id="7c07d-121">O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-121">A O.C.</span></span> <span data-ttu-id="7c07d-122">Nováková - AppreciateHub jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7c07d-122">Tanner - AppreciateHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7c07d-123">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7c07d-123">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7c07d-124">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7c07d-124">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7c07d-125">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7c07d-125">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7c07d-126">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c07d-126">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7c07d-127">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7c07d-127">Scenario description</span></span>
<span data-ttu-id="7c07d-128">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7c07d-128">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7c07d-129">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7c07d-129">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7c07d-130">Přidání O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-130">Adding O.C.</span></span> <span data-ttu-id="7c07d-131">Nováková - AppreciateHub z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7c07d-131">Tanner - AppreciateHub from hello gallery</span></span>
2. <span data-ttu-id="7c07d-132">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07d-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oc-tanner---appreciatehub-from-hello-gallery"></a><span data-ttu-id="7c07d-133">Přidání O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-133">Adding O.C.</span></span> <span data-ttu-id="7c07d-134">Nováková - AppreciateHub z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7c07d-134">Tanner - AppreciateHub from hello gallery</span></span>
<span data-ttu-id="7c07d-135">integrace hello tooconfigure O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-135">tooconfigure hello integration of O.C.</span></span> <span data-ttu-id="7c07d-136">Nováková - AppreciateHub do služby Azure AD, je nutné tooadd O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-136">Tanner - AppreciateHub into Azure AD, you need tooadd O.C.</span></span> <span data-ttu-id="7c07d-137">Nováková - AppreciateHub hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7c07d-137">Tanner - AppreciateHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7c07d-138">**tooadd O.C. Nováková - AppreciateHub z Galerie hello provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7c07d-138">**tooadd O.C. Tanner - AppreciateHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c07d-139">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7c07d-139">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7c07d-141">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-141">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7c07d-142">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-142">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7c07d-144">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7c07d-144">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7c07d-146">Hello vyhledávacího pole zadejte **O.C. Nováková - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-146">In hello search box, type **O.C. Tanner - AppreciateHub**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. <span data-ttu-id="7c07d-148">Na panelu výsledků hello vyberte **O.C. Nováková - AppreciateHub**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c07d-148">In hello results panel, select **O.C. Tanner - AppreciateHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7c07d-150">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07d-150">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7c07d-151">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-151">In this section, you configure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="7c07d-152">Nováková - AppreciateHub podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7c07d-152">Tanner - AppreciateHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7c07d-153">Pro toowork jeden přihlašování musí Azure AD tooknow hello příslušného uživatele v O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-153">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in O.C.</span></span> <span data-ttu-id="7c07d-154">Nováková - AppreciateHub je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7c07d-154">Tanner - AppreciateHub is tooa user in Azure AD.</span></span> <span data-ttu-id="7c07d-155">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-155">In other words, a link relationship between an Azure AD user and hello related user in O.C.</span></span> <span data-ttu-id="7c07d-156">Nováková - AppreciateHub musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7c07d-156">Tanner - AppreciateHub needs toobe established.</span></span>

<span data-ttu-id="7c07d-157">V O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-157">In O.C.</span></span> <span data-ttu-id="7c07d-158">Nováková - AppreciateHub, přiřaďte hello hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7c07d-158">Tanner - AppreciateHub, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7c07d-159">tooconfigure a testování Azure AD jednotné přihlašování s O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-159">tooconfigure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="7c07d-160">Nováková - AppreciateHub, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7c07d-160">Tanner - AppreciateHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7c07d-161">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7c07d-161">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7c07d-162">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7c07d-162">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7c07d-163">**[Vytváření O.C. Nováková - AppreciateHub testovacího uživatele](#creating-a-oc-tanner---appreciatehub-test-user)**  -toohave protějšek Britta Simon v O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-163">**[Creating a O.C. Tanner - AppreciateHub test user](#creating-a-oc-tanner---appreciatehub-test-user)** - toohave a counterpart of Britta Simon in O.C.</span></span> <span data-ttu-id="7c07d-164">Nováková - AppreciateHub, která je propojená toohello reprezentace uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7c07d-164">Tanner - AppreciateHub that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7c07d-165">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7c07d-165">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7c07d-166">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7c07d-166">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7c07d-167">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07d-167">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7c07d-168">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-168">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your O.C.</span></span> <span data-ttu-id="7c07d-169">Nováková - AppreciateHub aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c07d-169">Tanner - AppreciateHub application.</span></span>

<span data-ttu-id="7c07d-170">**tooconfigure Azure AD jednotné přihlašování s O.C. Nováková - AppreciateHub, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7c07d-170">**tooconfigure Azure AD single sign-on with O.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c07d-171">V portálu Azure, na hello hello **O.C. Nováková - AppreciateHub** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-171">In hello Azure portal, on hello **O.C. Tanner - AppreciateHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7c07d-173">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7c07d-173">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. <span data-ttu-id="7c07d-175">Na hello **O.C. Nováková - AppreciateHub domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7c07d-175">On hello **O.C. Tanner - AppreciateHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    <span data-ttu-id="7c07d-177">a.</span><span class="sxs-lookup"><span data-stu-id="7c07d-177">a.</span></span> <span data-ttu-id="7c07d-178">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span><span class="sxs-lookup"><span data-stu-id="7c07d-178">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7c07d-179">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="7c07d-179">This value is not real.</span></span> <span data-ttu-id="7c07d-180">Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7c07d-180">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="7c07d-181">Obraťte se na [O.C. Nováková - tým podpory AppreciateHub](mailto:sso@octanner.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7c07d-181">Contact [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) tooget this value.</span></span>

    <span data-ttu-id="7c07d-182">b.</span><span class="sxs-lookup"><span data-stu-id="7c07d-182">b.</span></span> <span data-ttu-id="7c07d-183">Soubor metadat otevřete hello pomocí hello následující odkaz: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span><span class="sxs-lookup"><span data-stu-id="7c07d-183">Open hello metadata file using hello following link: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span></span>
   
    <span data-ttu-id="7c07d-184">c.</span><span class="sxs-lookup"><span data-stu-id="7c07d-184">c.</span></span> <span data-ttu-id="7c07d-185">Vyhledejte hello **md:AssertionConsumerService** uzlu.</span><span class="sxs-lookup"><span data-stu-id="7c07d-185">Locate hello **md:AssertionConsumerService** node.</span></span> 
   
    <span data-ttu-id="7c07d-186">d.</span><span class="sxs-lookup"><span data-stu-id="7c07d-186">d.</span></span> <span data-ttu-id="7c07d-187">Zkopírujte hodnotu hello hello **umístění** atribut.</span><span class="sxs-lookup"><span data-stu-id="7c07d-187">Copy hello value of hello **Location** attribute.</span></span> 
   
    ![Konfigurovat nastavení aplikace][12]
   
    <span data-ttu-id="7c07d-189">e.</span><span class="sxs-lookup"><span data-stu-id="7c07d-189">e.</span></span> <span data-ttu-id="7c07d-190">V hello **přihlašovací adresa URL** textovému poli, po hello hodnoty, které jste získali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="7c07d-190">In hello **Sign On URL** textbox, past hello value you have obtained in hello previous step.</span></span>

4. <span data-ttu-id="7c07d-191">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7c07d-191">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. <span data-ttu-id="7c07d-193">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7c07d-193">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7c07d-195">tooconfigure jednotného přihlašování na **O.C. Nováková - AppreciateHub** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[O.C. Nováková - tým podpory AppreciateHub](mailto:sso@octanner.com).</span><span class="sxs-lookup"><span data-stu-id="7c07d-195">tooconfigure single sign-on on **O.C. Tanner - AppreciateHub** side, you need toosend hello downloaded **Metadata XML** too[O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com).</span></span>

> [!TIP]
> <span data-ttu-id="7c07d-196">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7c07d-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7c07d-197">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7c07d-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7c07d-198">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7c07d-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7c07d-199">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c07d-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="7c07d-200">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7c07d-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7c07d-202">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7c07d-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c07d-203">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7c07d-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7c07d-205">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7c07d-207">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7c07d-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7c07d-209">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7c07d-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7c07d-211">a.</span><span class="sxs-lookup"><span data-stu-id="7c07d-211">a.</span></span> <span data-ttu-id="7c07d-212">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7c07d-213">b.</span><span class="sxs-lookup"><span data-stu-id="7c07d-213">b.</span></span> <span data-ttu-id="7c07d-214">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7c07d-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7c07d-215">c.</span><span class="sxs-lookup"><span data-stu-id="7c07d-215">c.</span></span> <span data-ttu-id="7c07d-216">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7c07d-217">d.</span><span class="sxs-lookup"><span data-stu-id="7c07d-217">d.</span></span> <span data-ttu-id="7c07d-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-218">Click **Create**.</span></span>
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a><span data-ttu-id="7c07d-219">Vytváření O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-219">Creating a O.C.</span></span> <span data-ttu-id="7c07d-220">Nováková - AppreciateHub testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7c07d-220">Tanner - AppreciateHub test user</span></span>

<span data-ttu-id="7c07d-221">Hello cílem této části je toocreate volal Britta Simon v O.C. uživatele</span><span class="sxs-lookup"><span data-stu-id="7c07d-221">hello objective of this section is toocreate a user called Britta Simon in O.C.</span></span> <span data-ttu-id="7c07d-222">Nováková - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="7c07d-222">Tanner - AppreciateHub.</span></span>

<span data-ttu-id="7c07d-223">**toocreate uživatel volal Britta Simon v O.C. Nováková - AppreciateHub, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7c07d-223">**toocreate a user called Britta Simon in O.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

<span data-ttu-id="7c07d-224">Požádejte vaše [O.C. Nováková - tým podpory AppreciateHub](mailto:sso@octanner.com) toocreate uživatele, který má jako hello atribut nameID stejnou hodnotu jako uživatelské jméno hello Britta Simon ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7c07d-224">Ask your [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) toocreate a user that has as nameID attribute hello same value as hello user name of Britta Simon in Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7c07d-225">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7c07d-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7c07d-226">V této části povolíte tak, že udělíte přístup tooO.C toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7c07d-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooO.C.</span></span> <span data-ttu-id="7c07d-227">Nováková - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="7c07d-227">Tanner - AppreciateHub.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7c07d-229">**tooassign Britta Simon tooO.C. Nováková - AppreciateHub, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7c07d-229">**tooassign Britta Simon tooO.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c07d-230">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7c07d-232">V seznamu aplikace hello vyberte **O.C. Nováková - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-232">In hello applications list, select **O.C. Tanner - AppreciateHub**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. <span data-ttu-id="7c07d-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7c07d-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7c07d-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7c07d-236">Click **Add** button.</span></span> <span data-ttu-id="7c07d-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07d-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7c07d-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7c07d-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7c07d-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07d-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7c07d-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07d-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7c07d-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07d-242">Testing single sign-on</span></span>

<span data-ttu-id="7c07d-243">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7c07d-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="7c07d-244">Když kliknete na tlačítko hello O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-244">When you click hello O.C.</span></span> <span data-ttu-id="7c07d-245">Nováková - AppreciateHub dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour O.C.</span><span class="sxs-lookup"><span data-stu-id="7c07d-245">Tanner - AppreciateHub tile in hello Access Panel, you should get automatically signed-on tooyour O.C.</span></span> <span data-ttu-id="7c07d-246">Nováková - AppreciateHub aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c07d-246">Tanner - AppreciateHub application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c07d-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7c07d-247">Additional resources</span></span>

* [<span data-ttu-id="7c07d-248">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7c07d-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7c07d-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7c07d-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

