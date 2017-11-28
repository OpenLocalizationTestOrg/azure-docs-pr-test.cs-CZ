---
title: 'Kurz: Azure Active Directory integrace s ITRP | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ITRP."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 35463a55fcfc1e55c90700737961c1ff2e58992a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a><span data-ttu-id="00664-103">Kurz: Azure Active Directory integrace s ITRP</span><span class="sxs-lookup"><span data-stu-id="00664-103">Tutorial: Azure Active Directory integration with ITRP</span></span>

<span data-ttu-id="00664-104">V tomto kurzu zjistíte, jak toointegrate ITRP s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="00664-104">In this tutorial, you learn how toointegrate ITRP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="00664-105">Integrace ITRP s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="00664-105">Integrating ITRP with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="00664-106">Můžete řídit ve službě Azure AD, který má přístup tooITRP</span><span class="sxs-lookup"><span data-stu-id="00664-106">You can control in Azure AD who has access tooITRP</span></span>
- <span data-ttu-id="00664-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooITRP (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="00664-107">You can enable your users tooautomatically get signed-on tooITRP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="00664-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="00664-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="00664-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="00664-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00664-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="00664-110">Prerequisites</span></span>

<span data-ttu-id="00664-111">Integrace služby Azure AD s ITRP tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="00664-111">tooconfigure Azure AD integration with ITRP, you need hello following items:</span></span>

- <span data-ttu-id="00664-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="00664-112">An Azure AD subscription</span></span>
- <span data-ttu-id="00664-113">ITRP jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="00664-113">An ITRP single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="00664-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="00664-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="00664-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="00664-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="00664-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="00664-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="00664-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00664-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="00664-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="00664-118">Scenario description</span></span>
<span data-ttu-id="00664-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="00664-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="00664-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="00664-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="00664-121">Přidání ITRP z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="00664-121">Adding ITRP from hello gallery</span></span>
2. <span data-ttu-id="00664-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="00664-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itrp-from-hello-gallery"></a><span data-ttu-id="00664-123">Přidání ITRP z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="00664-123">Adding ITRP from hello gallery</span></span>
<span data-ttu-id="00664-124">tooconfigure hello integrace ITRP v tooAzure AD, je nutné tooadd ITRP hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="00664-124">tooconfigure hello integration of ITRP in tooAzure AD, you need tooadd ITRP from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="00664-125">**tooadd ITRP z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="00664-125">**tooadd ITRP from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="00664-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="00664-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="00664-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="00664-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="00664-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="00664-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="00664-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="00664-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="00664-133">Hello vyhledávacího pole zadejte **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="00664-133">In hello search box, type **ITRP**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_search.png)

5. <span data-ttu-id="00664-135">Na panelu výsledků hello vyberte **ITRP**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="00664-135">In hello results panel, select **ITRP**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="00664-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="00664-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="00664-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ITRP podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="00664-138">In this section, you configure and test Azure AD single sign-on with ITRP based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="00664-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ITRP je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00664-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ITRP is tooa user in Azure AD.</span></span> <span data-ttu-id="00664-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ITRP musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="00664-140">In other words, a link relationship between an Azure AD user and hello related user in ITRP needs toobe established.</span></span>

<span data-ttu-id="00664-141">V ITRP, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="00664-141">In ITRP, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="00664-142">tooconfigure a testu Azure AD jednotné přihlašování s ITRP, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="00664-142">tooconfigure and test Azure AD single sign-on with ITRP, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="00664-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="00664-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="00664-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="00664-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="00664-145">**[Vytvoření ITRP testovací uživatel](#creating-an-itrp-test-user)**  -toohave protějšek Britta Simon v ITRP, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="00664-145">**[Creating an ITRP test user](#creating-an-itrp-test-user)** - toohave a counterpart of Britta Simon in ITRP that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="00664-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="00664-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="00664-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="00664-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="00664-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="00664-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="00664-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ITRP.</span><span class="sxs-lookup"><span data-stu-id="00664-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ITRP application.</span></span>

<span data-ttu-id="00664-150">**tooconfigure Azure AD jednotné přihlašování s ITRP, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="00664-150">**tooconfigure Azure AD single sign-on with ITRP, perform hello following steps:**</span></span>

1. <span data-ttu-id="00664-151">V portálu Azure, na hello hello **ITRP** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="00664-151">In hello Azure portal, on hello **ITRP** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="00664-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="00664-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_samlbase.png)

3. <span data-ttu-id="00664-155">Na hello **ITRP domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="00664-155">On hello **ITRP Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_url.png)

    <span data-ttu-id="00664-157">a.</span><span class="sxs-lookup"><span data-stu-id="00664-157">a.</span></span> <span data-ttu-id="00664-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="00664-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.itrp.com`</span></span>

    <span data-ttu-id="00664-159">b.</span><span class="sxs-lookup"><span data-stu-id="00664-159">b.</span></span> <span data-ttu-id="00664-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="00664-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.itrp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="00664-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="00664-161">These values are not real.</span></span> <span data-ttu-id="00664-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="00664-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="00664-163">Obraťte se na [tým podpory ITRP klienta](https://www.itrp.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="00664-163">Contact [ITRP Client support team](https://www.itrp.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="00664-164">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="00664-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_certificate.png) 

5. <span data-ttu-id="00664-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="00664-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="00664-168">Na hello **ITRP konfigurace** klikněte na tlačítko **konfigurace ITRP** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="00664-168">On hello **ITRP Configuration** section, click **Configure ITRP** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="00664-169">Kopírování hello **SAML jednu přihlašování služby adresu URL a adresy URL Sign-Out** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="00664-169">Copy hello **SAML Single Sign-On Service URL and Sign-Out URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_configure.png) 

7. <span data-ttu-id="00664-171">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti ITRP tooyour.</span><span class="sxs-lookup"><span data-stu-id="00664-171">In a different web browser window, log in tooyour ITRP company site as an administrator.</span></span>

8. <span data-ttu-id="00664-172">V panelu nástrojů hello hello nahoře, klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="00664-172">In hello toolbar on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="00664-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span><span class="sxs-lookup"><span data-stu-id="00664-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span></span>

8. <span data-ttu-id="00664-174">V levém navigačním podokně hello vyberte **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="00664-174">In hello left navigation pane, select **Single Sign-On**.</span></span>
   
    <span data-ttu-id="00664-175">![Jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/ic775571.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="00664-175">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775571.png "Single Sign-On")</span></span>

9. <span data-ttu-id="00664-176">V hello jednotné přihlašování v konfiguračním oddílu proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="00664-176">In hello Single Sign-On configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="00664-177">![Jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/ic775572.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="00664-177">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775572.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="00664-178">![Jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/ic775573.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="00664-178">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775573.png "Single Sign-On")</span></span>   

    <span data-ttu-id="00664-179">a.</span><span class="sxs-lookup"><span data-stu-id="00664-179">a.</span></span> <span data-ttu-id="00664-180">Klikněte na **Povolit**.</span><span class="sxs-lookup"><span data-stu-id="00664-180">Click **Enable**.</span></span>

    <span data-ttu-id="00664-181">b.</span><span class="sxs-lookup"><span data-stu-id="00664-181">b.</span></span> <span data-ttu-id="00664-182">V **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="00664-182">In **Remote Log Out URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="00664-183">c.</span><span class="sxs-lookup"><span data-stu-id="00664-183">c.</span></span> <span data-ttu-id="00664-184">V **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="00664-184">In **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="00664-185">d.In **otisků prstů certifikátů** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="00664-185">d.In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span> 
      
10. <span data-ttu-id="00664-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="00664-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="00664-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="00664-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="00664-188">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="00664-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="00664-189">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="00664-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="00664-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="00664-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="00664-191">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="00664-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="00664-193">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="00664-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="00664-194">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="00664-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="00664-196">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="00664-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="00664-198">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="00664-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="00664-200">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="00664-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="00664-202">a.</span><span class="sxs-lookup"><span data-stu-id="00664-202">a.</span></span> <span data-ttu-id="00664-203">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="00664-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="00664-204">b.</span><span class="sxs-lookup"><span data-stu-id="00664-204">b.</span></span> <span data-ttu-id="00664-205">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="00664-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="00664-206">c.</span><span class="sxs-lookup"><span data-stu-id="00664-206">c.</span></span> <span data-ttu-id="00664-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="00664-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="00664-208">d.</span><span class="sxs-lookup"><span data-stu-id="00664-208">d.</span></span> <span data-ttu-id="00664-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="00664-209">Click **Create**.</span></span>
 
### <a name="creating-an-itrp-test-user"></a><span data-ttu-id="00664-210">Vytváření testovacího uživatele ITRP</span><span class="sxs-lookup"><span data-stu-id="00664-210">Creating an ITRP test user</span></span>

<span data-ttu-id="00664-211">Uživatelé toolog tooenable Azure AD v tooITRP, se musí být zřízená v tooITRP.</span><span class="sxs-lookup"><span data-stu-id="00664-211">tooenable Azure AD users toolog in tooITRP, they must be provisioned in tooITRP.</span></span>  

<span data-ttu-id="00664-212">V případě hello ITRP zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="00664-212">In hello case of ITRP, provisioning is a manual task.</span></span>

<span data-ttu-id="00664-213">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="00664-213">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="00664-214">Přihlaste se tooyour **ITRP** klienta.</span><span class="sxs-lookup"><span data-stu-id="00664-214">Log in tooyour **ITRP** tenant.</span></span>

2. <span data-ttu-id="00664-215">V panelu nástrojů hello hello nahoře, klikněte na **záznamy**.</span><span class="sxs-lookup"><span data-stu-id="00664-215">In hello toolbar on hello top, click **Records**.</span></span>
   
    <span data-ttu-id="00664-216">![Správce](./media/active-directory-saas-itrp-tutorial/ic775575.png "správce")</span><span class="sxs-lookup"><span data-stu-id="00664-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span></span>

3. <span data-ttu-id="00664-217">Hello místní nabídce vyberte **osoby**.</span><span class="sxs-lookup"><span data-stu-id="00664-217">From hello popup menu, select **People**.</span></span>
   
    <span data-ttu-id="00664-218">![Lidé](./media/active-directory-saas-itrp-tutorial/ic775587.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="00664-218">![People](./media/active-directory-saas-itrp-tutorial/ic775587.png "People")</span></span>

4. <span data-ttu-id="00664-219">Klikněte na tlačítko **přidat nové osobě** ("+").</span><span class="sxs-lookup"><span data-stu-id="00664-219">Click **Add New Person** (“+”).</span></span>
   
    <span data-ttu-id="00664-220">![Správce](./media/active-directory-saas-itrp-tutorial/ic775576.png "správce")</span><span class="sxs-lookup"><span data-stu-id="00664-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span></span>

5. <span data-ttu-id="00664-221">V dialogovém okně Přidat nové osobě hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="00664-221">On hello Add New Person dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="00664-222">![Uživatel](./media/active-directory-saas-itrp-tutorial/ic775577.png "uživatele")</span><span class="sxs-lookup"><span data-stu-id="00664-222">![User](./media/active-directory-saas-itrp-tutorial/ic775577.png "User")</span></span> 
      
    <span data-ttu-id="00664-223">a.</span><span class="sxs-lookup"><span data-stu-id="00664-223">a.</span></span> <span data-ttu-id="00664-224">Typ hello **název**, **e-mailu** platného účtu AAD chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="00664-224">Type hello **Name**, **Email** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="00664-225">b.</span><span class="sxs-lookup"><span data-stu-id="00664-225">b.</span></span> <span data-ttu-id="00664-226">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="00664-226">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="00664-227">Můžete použít všechny ostatní ITRP uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ITRP tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="00664-227">You can use any other ITRP user account creation tools or APIs provided by ITRP tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="00664-228">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="00664-228">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="00664-229">V této části povolíte tak, že udělíte přístup tooITRP toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="00664-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooITRP.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="00664-231">**tooassign Britta Simon tooITRP, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="00664-231">**tooassign Britta Simon tooITRP, perform hello following steps:**</span></span>

1. <span data-ttu-id="00664-232">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="00664-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="00664-234">V seznamu aplikace hello vyberte **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="00664-234">In hello applications list, select **ITRP**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_app.png) 

3. <span data-ttu-id="00664-236">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="00664-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="00664-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="00664-238">Click **Add** button.</span></span> <span data-ttu-id="00664-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="00664-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="00664-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="00664-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="00664-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="00664-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="00664-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="00664-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="00664-244">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="00664-244">Testing single sign-on</span></span>

<span data-ttu-id="00664-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="00664-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="00664-246">Po kliknutí na tlačítko hello ITRP dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ITRP aplikace.</span><span class="sxs-lookup"><span data-stu-id="00664-246">When you click hello ITRP tile in hello Access Panel, you should get automatically signed-on tooyour ITRP application.</span></span>
<span data-ttu-id="00664-247">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="00664-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00664-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="00664-248">Additional resources</span></span>

* [<span data-ttu-id="00664-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="00664-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00664-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="00664-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_203.png

