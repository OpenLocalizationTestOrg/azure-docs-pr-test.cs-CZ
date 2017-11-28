---
title: 'Kurz: Azure Active Directory integrace s Freshservice | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Freshservice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d73624b87d058f66885ae72fda69a0aacc89c1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a><span data-ttu-id="d6085-103">Kurz: Azure Active Directory integrace s Freshservice</span><span class="sxs-lookup"><span data-stu-id="d6085-103">Tutorial: Azure Active Directory integration with Freshservice</span></span>

<span data-ttu-id="d6085-104">V tomto kurzu zjistíte, jak toointegrate Freshservice s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d6085-104">In this tutorial, you learn how toointegrate Freshservice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d6085-105">Integrace Freshservice s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d6085-105">Integrating Freshservice with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d6085-106">Můžete řídit ve službě Azure AD, který má přístup tooFreshservice</span><span class="sxs-lookup"><span data-stu-id="d6085-106">You can control in Azure AD who has access tooFreshservice</span></span>
- <span data-ttu-id="d6085-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFreshservice (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6085-107">You can enable your users tooautomatically get signed-on tooFreshservice (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d6085-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d6085-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d6085-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d6085-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6085-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d6085-110">Prerequisites</span></span>

<span data-ttu-id="d6085-111">Integrace služby Azure AD s Freshservice tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d6085-111">tooconfigure Azure AD integration with Freshservice, you need hello following items:</span></span>

- <span data-ttu-id="d6085-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6085-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d6085-113">Freshservice jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d6085-113">A Freshservice single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d6085-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d6085-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d6085-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d6085-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d6085-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d6085-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d6085-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d6085-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d6085-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d6085-118">Scenario description</span></span>
<span data-ttu-id="d6085-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d6085-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d6085-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d6085-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d6085-121">Přidání Freshservice z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d6085-121">Adding Freshservice from hello gallery</span></span>
2. <span data-ttu-id="d6085-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d6085-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshservice-from-hello-gallery"></a><span data-ttu-id="d6085-123">Přidání Freshservice z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d6085-123">Adding Freshservice from hello gallery</span></span>
<span data-ttu-id="d6085-124">tooconfigure hello integrace Freshservice do Azure AD, je nutné tooadd Freshservice hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d6085-124">tooconfigure hello integration of Freshservice into Azure AD, you need tooadd Freshservice from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d6085-125">**tooadd Freshservice z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d6085-125">**tooadd Freshservice from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6085-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d6085-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d6085-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d6085-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d6085-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d6085-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d6085-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d6085-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d6085-133">Hello vyhledávacího pole zadejte **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="d6085-133">In hello search box, type **Freshservice**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. <span data-ttu-id="d6085-135">Na panelu výsledků hello vyberte **Freshservice**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6085-135">In hello results panel, select **Freshservice**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d6085-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d6085-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d6085-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Freshservice podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d6085-138">In this section, you configure and test Azure AD single sign-on with Freshservice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d6085-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Freshservice je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d6085-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Freshservice is tooa user in Azure AD.</span></span> <span data-ttu-id="d6085-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Freshservice musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d6085-140">In other words, a link relationship between an Azure AD user and hello related user in Freshservice needs toobe established.</span></span>

<span data-ttu-id="d6085-141">V Freshservice, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d6085-141">In Freshservice, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d6085-142">tooconfigure a testu Azure AD jednotné přihlašování s Freshservice, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d6085-142">tooconfigure and test Azure AD single sign-on with Freshservice, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d6085-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d6085-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d6085-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d6085-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d6085-145">**[Vytvoření zkušebního uživatele Freshservice](#creating-a-freshservice-test-user)**  -toohave protějšek Britta Simon v Freshservice, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6085-145">**[Creating a Freshservice test user](#creating-a-freshservice-test-user)** - toohave a counterpart of Britta Simon in Freshservice that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d6085-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d6085-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d6085-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d6085-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d6085-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d6085-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d6085-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Freshservice.</span><span class="sxs-lookup"><span data-stu-id="d6085-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Freshservice application.</span></span>

<span data-ttu-id="d6085-150">**tooconfigure Azure AD jednotné přihlašování s Freshservice, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d6085-150">**tooconfigure Azure AD single sign-on with Freshservice, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6085-151">V portálu Azure, na hello hello **Freshservice** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d6085-151">In hello Azure portal, on hello **Freshservice** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d6085-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d6085-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. <span data-ttu-id="d6085-155">Na hello **Freshservice domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d6085-155">On hello **Freshservice Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    <span data-ttu-id="d6085-157">a.</span><span class="sxs-lookup"><span data-stu-id="d6085-157">a.</span></span> <span data-ttu-id="d6085-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="d6085-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<democompany>.freshservice.com`</span></span>

    <span data-ttu-id="d6085-159">b.</span><span class="sxs-lookup"><span data-stu-id="d6085-159">b.</span></span> <span data-ttu-id="d6085-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="d6085-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<democompany>.freshservice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d6085-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d6085-161">These values are not real.</span></span> <span data-ttu-id="d6085-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d6085-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d6085-163">Obraťte se na [tým podpory Freshservice klienta](https://support.freshservice.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d6085-163">Contact [Freshservice Client support team](https://support.freshservice.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="d6085-164">Na hello **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d6085-164">On hello **SAML Signing Certificate** section, copy **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. <span data-ttu-id="d6085-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d6085-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d6085-168">Na hello **Freshservice konfigurace** klikněte na tlačítko **konfigurace Freshservice** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d6085-168">On hello **Freshservice Configuration** section, click **Configure Freshservice** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d6085-169">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d6085-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. <span data-ttu-id="d6085-171">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Freshservice tooyour.</span><span class="sxs-lookup"><span data-stu-id="d6085-171">In a different web browser window, log in tooyour Freshservice company site as an administrator.</span></span>

8. <span data-ttu-id="d6085-172">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="d6085-172">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="d6085-173">![Správce](./media/active-directory-saas-freshservice-tutorial/ic790814.png "správce")</span><span class="sxs-lookup"><span data-stu-id="d6085-173">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

9. <span data-ttu-id="d6085-174">V hello **zákaznický portál**, klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="d6085-174">In hello **Customer Portal**, click **Security**.</span></span>
   
    <span data-ttu-id="d6085-175">![Zabezpečení](./media/active-directory-saas-freshservice-tutorial/ic790815.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="d6085-175">![Security](./media/active-directory-saas-freshservice-tutorial/ic790815.png "Security")</span></span>

10. <span data-ttu-id="d6085-176">V hello **zabezpečení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d6085-176">In hello **Security** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d6085-177">![Jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/ic790816.png "jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="d6085-177">![Single Sign On](./media/active-directory-saas-freshservice-tutorial/ic790816.png "Single Sign On")</span></span>
   
    <span data-ttu-id="d6085-178">a.</span><span class="sxs-lookup"><span data-stu-id="d6085-178">a.</span></span> <span data-ttu-id="d6085-179">Přepínač **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d6085-179">Switch **Single Sign On**.</span></span>

    <span data-ttu-id="d6085-180">b.</span><span class="sxs-lookup"><span data-stu-id="d6085-180">b.</span></span> <span data-ttu-id="d6085-181">Vyberte **jednotné přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="d6085-181">Select **SAML SSO**.</span></span>

    <span data-ttu-id="d6085-182">c.</span><span class="sxs-lookup"><span data-stu-id="d6085-182">c.</span></span> <span data-ttu-id="d6085-183">V hello **SAML přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d6085-183">In hello **SAML Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d6085-184">d.</span><span class="sxs-lookup"><span data-stu-id="d6085-184">d.</span></span> <span data-ttu-id="d6085-185">V hello **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d6085-185">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d6085-186">e.</span><span class="sxs-lookup"><span data-stu-id="d6085-186">e.</span></span> <span data-ttu-id="d6085-187">V **otisků certifikátu zabezpečení** textovému poli, vložte hello **kryptografický OTISK** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d6085-187">In **Security Certificate Fingerprint** textbox, paste hello **THUMBPRINT** value of certificate which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d6085-188">f.</span><span class="sxs-lookup"><span data-stu-id="d6085-188">f.</span></span> <span data-ttu-id="d6085-189">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="d6085-189">Click **Save**</span></span>
   
> [!TIP]
> <span data-ttu-id="d6085-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d6085-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d6085-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d6085-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d6085-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d6085-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d6085-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6085-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="d6085-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d6085-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d6085-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d6085-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6085-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d6085-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d6085-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d6085-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d6085-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="d6085-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d6085-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d6085-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d6085-205">a.</span><span class="sxs-lookup"><span data-stu-id="d6085-205">a.</span></span> <span data-ttu-id="d6085-206">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d6085-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d6085-207">b.</span><span class="sxs-lookup"><span data-stu-id="d6085-207">b.</span></span> <span data-ttu-id="d6085-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d6085-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d6085-209">c.</span><span class="sxs-lookup"><span data-stu-id="d6085-209">c.</span></span> <span data-ttu-id="d6085-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d6085-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d6085-211">d.</span><span class="sxs-lookup"><span data-stu-id="d6085-211">d.</span></span> <span data-ttu-id="d6085-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d6085-212">Click **Create**.</span></span>
 
### <a name="creating-a-freshservice-test-user"></a><span data-ttu-id="d6085-213">Vytvoření zkušebního uživatele Freshservice</span><span class="sxs-lookup"><span data-stu-id="d6085-213">Creating a Freshservice test user</span></span>

<span data-ttu-id="d6085-214">Uživatelé toolog tooenable Azure AD v tooFreshService, se musí být zřízená do FreshService.</span><span class="sxs-lookup"><span data-stu-id="d6085-214">tooenable Azure AD users toolog in tooFreshService, they must be provisioned into FreshService.</span></span> <span data-ttu-id="d6085-215">V případě hello FreshService zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d6085-215">In hello case of FreshService, provisioning is a manual task.</span></span>

<span data-ttu-id="d6085-216">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d6085-216">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6085-217">Přihlaste se tooyour **FreshService** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="d6085-217">Log in tooyour **FreshService** company site as an administrator.</span></span>

2. <span data-ttu-id="d6085-218">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="d6085-218">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="d6085-219">![Správce](./media/active-directory-saas-freshservice-tutorial/ic790814.png "správce")</span><span class="sxs-lookup"><span data-stu-id="d6085-219">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

3. <span data-ttu-id="d6085-220">V hello **Správa uživatelů** klikněte na tlačítko **žadatelů o**.</span><span class="sxs-lookup"><span data-stu-id="d6085-220">In hello **User Management** section, click **Requesters**.</span></span>
   
    <span data-ttu-id="d6085-221">![Žadatelů o](./media/active-directory-saas-freshservice-tutorial/ic790818.png "žadatelů o")</span><span class="sxs-lookup"><span data-stu-id="d6085-221">![Requesters](./media/active-directory-saas-freshservice-tutorial/ic790818.png "Requesters")</span></span>

4. <span data-ttu-id="d6085-222">Klikněte na tlačítko **nový žadatel**.</span><span class="sxs-lookup"><span data-stu-id="d6085-222">Click **New Requester**.</span></span>
   
    <span data-ttu-id="d6085-223">![Nové žadatelů o](./media/active-directory-saas-freshservice-tutorial/ic790819.png "nové žadatelů o")</span><span class="sxs-lookup"><span data-stu-id="d6085-223">![New Requesters](./media/active-directory-saas-freshservice-tutorial/ic790819.png "New Requesters")</span></span>

5. <span data-ttu-id="d6085-224">V hello **nový žadatel** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d6085-224">In hello **New Requester** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d6085-225">![Nový žadatel](./media/active-directory-saas-freshservice-tutorial/ic790820.png "nový žadatel")</span><span class="sxs-lookup"><span data-stu-id="d6085-225">![New Requester](./media/active-directory-saas-freshservice-tutorial/ic790820.png "New Requester")</span></span>   

    <span data-ttu-id="d6085-226">a.</span><span class="sxs-lookup"><span data-stu-id="d6085-226">a.</span></span> <span data-ttu-id="d6085-227">Zadejte hello **křestní jméno** a **e-mailu** atributy platný účet služby Azure Active Directory do hello chcete tooprovision související textových polí.</span><span class="sxs-lookup"><span data-stu-id="d6085-227">Enter hello **First Name** and **Email** attributes of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="d6085-228">b.</span><span class="sxs-lookup"><span data-stu-id="d6085-228">b.</span></span> <span data-ttu-id="d6085-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d6085-229">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="d6085-230">Držitel účtu Azure Active Directory Hello získá e-mailu, včetně účet odkaz tooconfirm hello předtím, než se stane aktivní</span><span class="sxs-lookup"><span data-stu-id="d6085-230">hello Azure Active Directory account holder gets an email including a link tooconfirm hello account before it becomes active</span></span>
    >  

>[!NOTE]
><span data-ttu-id="d6085-231">Můžete použít všechny ostatní FreshService uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované FreshService tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d6085-231">You can use any other FreshService user account creation tools or APIs provided by FreshService tooprovision AAD user accounts.</span></span>
>  

![Přiřadit uživatele][200] 

<span data-ttu-id="d6085-233">**tooassign Britta Simon tooFreshservice, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d6085-233">**tooassign Britta Simon tooFreshservice, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6085-234">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d6085-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d6085-236">V seznamu aplikace hello vyberte **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="d6085-236">In hello applications list, select **Freshservice**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. <span data-ttu-id="d6085-238">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d6085-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d6085-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d6085-240">Click **Add** button.</span></span> <span data-ttu-id="d6085-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d6085-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d6085-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d6085-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d6085-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d6085-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d6085-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d6085-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d6085-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d6085-246">Testing single sign-on</span></span>

<span data-ttu-id="d6085-247">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d6085-247">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d6085-248">Když kliknete na dlaždici Freshservice hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Freshservice aplikace.</span><span class="sxs-lookup"><span data-stu-id="d6085-248">When you click hello Freshservice tile in hello Access Panel, you should get automatically signed-on tooyour Freshservice application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6085-249">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d6085-249">Additional resources</span></span>

* [<span data-ttu-id="d6085-250">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d6085-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d6085-251">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d6085-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

