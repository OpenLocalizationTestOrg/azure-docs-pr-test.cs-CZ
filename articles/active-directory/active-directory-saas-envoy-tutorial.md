---
title: 'Kurz: Azure Active Directory integrace s Envoy | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Envoy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71f7afcc-1033-4098-9b7e-4f9f2b26f734
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 93add7c1f3cf1fc163acc505f11e34bd696c571c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-envoy"></a><span data-ttu-id="5922a-103">Kurz: Azure Active Directory integrace s Envoy</span><span class="sxs-lookup"><span data-stu-id="5922a-103">Tutorial: Azure Active Directory integration with Envoy</span></span>

<span data-ttu-id="5922a-104">V tomto kurzu zjistíte, jak toointegrate Envoy službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5922a-104">In this tutorial, you learn how toointegrate Envoy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5922a-105">Integrace Envoy s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5922a-105">Integrating Envoy with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5922a-106">Můžete ovládat ve službě Azure AD, který má přístup tooEnvoy.</span><span class="sxs-lookup"><span data-stu-id="5922a-106">You can control in Azure AD who has access tooEnvoy.</span></span>
- <span data-ttu-id="5922a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEnvoy (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5922a-107">You can enable your users tooautomatically get signed-on tooEnvoy (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="5922a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5922a-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="5922a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5922a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5922a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5922a-110">Prerequisites</span></span>

<span data-ttu-id="5922a-111">Integrace služby Azure AD s Envoy tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5922a-111">tooconfigure Azure AD integration with Envoy, you need hello following items:</span></span>

- <span data-ttu-id="5922a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5922a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5922a-113">Envoy jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5922a-113">An Envoy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5922a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5922a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5922a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5922a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5922a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5922a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5922a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5922a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5922a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5922a-118">Scenario description</span></span>
<span data-ttu-id="5922a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5922a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5922a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5922a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5922a-121">Přidání Envoy z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5922a-121">Adding Envoy from hello gallery</span></span>
2. <span data-ttu-id="5922a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5922a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-envoy-from-hello-gallery"></a><span data-ttu-id="5922a-123">Přidání Envoy z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5922a-123">Adding Envoy from hello gallery</span></span>
<span data-ttu-id="5922a-124">tooconfigure hello integrace Envoy do Azure AD, je nutné tooadd Envoy hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5922a-124">tooconfigure hello integration of Envoy into Azure AD, you need tooadd Envoy from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5922a-125">**tooadd Envoy z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5922a-125">**tooadd Envoy from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5922a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5922a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="5922a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5922a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5922a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5922a-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="5922a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5922a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="5922a-133">Hello vyhledávacího pole zadejte **Envoy**, vyberte **Envoy** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5922a-133">In hello search box, type **Envoy**, select **Envoy** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Envoy v seznamu výsledků hello](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5922a-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5922a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="5922a-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Envoy podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5922a-136">In this section, you configure and test Azure AD single sign-on with Envoy based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5922a-137">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Envoy je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5922a-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Envoy is tooa user in Azure AD.</span></span> <span data-ttu-id="5922a-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Envoy musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5922a-138">In other words, a link relationship between an Azure AD user and hello related user in Envoy needs toobe established.</span></span>

<span data-ttu-id="5922a-139">V Envoy, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5922a-139">In Envoy, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5922a-140">tooconfigure a testu Azure AD jednotné přihlašování s Envoy, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5922a-140">tooconfigure and test Azure AD single sign-on with Envoy, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5922a-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5922a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5922a-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5922a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5922a-143">**[Vytvořit testovací uživatele s Envoy](#create-an-envoy-test-user)**  -toohave protějšek Britta Simon v Envoy, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5922a-143">**[Create an Envoy test user](#create-an-envoy-test-user)** - toohave a counterpart of Britta Simon in Envoy that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5922a-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5922a-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5922a-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5922a-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5922a-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5922a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5922a-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Envoy.</span><span class="sxs-lookup"><span data-stu-id="5922a-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Envoy application.</span></span>

<span data-ttu-id="5922a-148">**tooconfigure Azure AD jednotné přihlašování s Envoy, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5922a-148">**tooconfigure Azure AD single sign-on with Envoy, perform hello following steps:**</span></span>

1. <span data-ttu-id="5922a-149">V portálu Azure, na hello hello **Envoy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5922a-149">In hello Azure portal, on hello **Envoy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="5922a-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5922a-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_samlbase.png)

3. <span data-ttu-id="5922a-153">Na hello **Envoy domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5922a-153">On hello **Envoy Domain and URLs** section, perform hello following steps:</span></span>

    ![Envoy domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_url.png)

    <span data-ttu-id="5922a-155">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.Envoy.com`</span><span class="sxs-lookup"><span data-stu-id="5922a-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Envoy.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="5922a-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="5922a-156">This value is not real.</span></span> <span data-ttu-id="5922a-157">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5922a-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="5922a-158">Obraťte se na [tým podpory Envoy klienta](https://envoy.com/contact/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5922a-158">Contact [Envoy Client support team](https://envoy.com/contact/) tooget this value.</span></span>

4. <span data-ttu-id="5922a-159">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu...</span><span class="sxs-lookup"><span data-stu-id="5922a-159">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate..</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_certificate.png) 

5. <span data-ttu-id="5922a-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5922a-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-envoy-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5922a-163">Na hello **Envoy konfigurace** klikněte na tlačítko **konfigurace Envoy** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5922a-163">On hello **Envoy Configuration** section, click **Configure Envoy** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5922a-164">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5922a-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Envoy](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_configure.png)

7. <span data-ttu-id="5922a-166">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Envoy.</span><span class="sxs-lookup"><span data-stu-id="5922a-166">In a different web browser window, log into your Envoy company site as an administrator.</span></span>

8. <span data-ttu-id="5922a-167">V panelu nástrojů hello hello nahoře, klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5922a-167">In hello toolbar on hello top, click **Settings**.</span></span>

    <span data-ttu-id="5922a-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span><span class="sxs-lookup"><span data-stu-id="5922a-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span></span>

9. <span data-ttu-id="5922a-169">Klikněte na tlačítko **společnosti**.</span><span class="sxs-lookup"><span data-stu-id="5922a-169">Click **Company**.</span></span>

    <span data-ttu-id="5922a-170">![Společnosti](./media/active-directory-saas-envoy-tutorial/ic776783.png "společnosti")</span><span class="sxs-lookup"><span data-stu-id="5922a-170">![Company](./media/active-directory-saas-envoy-tutorial/ic776783.png "Company")</span></span>

10. <span data-ttu-id="5922a-171">Klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="5922a-171">Click **SAML**.</span></span>

    <span data-ttu-id="5922a-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="5922a-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span></span>

11. <span data-ttu-id="5922a-173">V hello **ověřování SAML** konfigurace části, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5922a-173">In hello **SAML Authentication** configuration section, perform hello following steps:</span></span>

    <span data-ttu-id="5922a-174">![Ověřování SAML](./media/active-directory-saas-envoy-tutorial/ic776785.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="5922a-174">![SAML authentication](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML authentication")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="5922a-175">Hello hodnotou pro ID umístění Ústředí hello je automaticky generovány hello aplikací.</span><span class="sxs-lookup"><span data-stu-id="5922a-175">hello value for hello HQ location ID is auto generated by hello application.</span></span>
    
    <span data-ttu-id="5922a-176">a.</span><span class="sxs-lookup"><span data-stu-id="5922a-176">a.</span></span> <span data-ttu-id="5922a-177">V **otisk prstu** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5922a-177">In **Fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="5922a-178">b.</span><span class="sxs-lookup"><span data-stu-id="5922a-178">b.</span></span> <span data-ttu-id="5922a-179">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali formuláři hello Azure portálu do hello **adresa URL IDENTITY zprostředkovatele protokolu HTTP SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5922a-179">Paste **SAML Single Sign-On Service URL** value, which you have copied form hello Azure portal into hello **IDENTITY PROVIDER HTTP SAML URL** textbox.</span></span>
    
    <span data-ttu-id="5922a-180">c.</span><span class="sxs-lookup"><span data-stu-id="5922a-180">c.</span></span> <span data-ttu-id="5922a-181">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="5922a-181">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="5922a-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5922a-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5922a-183">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5922a-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5922a-184">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5922a-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5922a-185">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5922a-185">Create an Azure AD test user</span></span>

<span data-ttu-id="5922a-186">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5922a-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="5922a-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5922a-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5922a-189">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5922a-189">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-envoy-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="5922a-191">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5922a-191">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-envoy-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="5922a-193">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5922a-193">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-envoy-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="5922a-195">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5922a-195">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-envoy-tutorial/create_aaduser_04.png)

    <span data-ttu-id="5922a-197">a.</span><span class="sxs-lookup"><span data-stu-id="5922a-197">a.</span></span> <span data-ttu-id="5922a-198">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5922a-198">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5922a-199">b.</span><span class="sxs-lookup"><span data-stu-id="5922a-199">b.</span></span> <span data-ttu-id="5922a-200">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5922a-200">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="5922a-201">c.</span><span class="sxs-lookup"><span data-stu-id="5922a-201">c.</span></span> <span data-ttu-id="5922a-202">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="5922a-202">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="5922a-203">d.</span><span class="sxs-lookup"><span data-stu-id="5922a-203">d.</span></span> <span data-ttu-id="5922a-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5922a-204">Click **Create**.</span></span>
 
### <a name="create-an-envoy-test-user"></a><span data-ttu-id="5922a-205">Vytvořit uživatele s Envoy testu</span><span class="sxs-lookup"><span data-stu-id="5922a-205">Create an Envoy test user</span></span>

<span data-ttu-id="5922a-206">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooEnvoy.</span><span class="sxs-lookup"><span data-stu-id="5922a-206">There is no action item for you tooconfigure user provisioning tooEnvoy.</span></span> <span data-ttu-id="5922a-207">Když se uživatel s přiřazenou pokusí toolog do Envoy pomocí hello přístupového panelu, Envoy ověří, zda existuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5922a-207">When an assigned user tries toolog into Envoy using hello access panel, Envoy checks whether hello user exists.</span></span> <span data-ttu-id="5922a-208">Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří pomocí Envoy.</span><span class="sxs-lookup"><span data-stu-id="5922a-208">If there is no user account available yet, it is automatically created by Envoy.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5922a-209">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5922a-209">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5922a-210">V této části povolíte tak, že udělíte přístup tooEnvoy toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5922a-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEnvoy.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="5922a-212">**tooassign Britta Simon tooEnvoy, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5922a-212">**tooassign Britta Simon tooEnvoy, perform hello following steps:**</span></span>

1. <span data-ttu-id="5922a-213">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5922a-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5922a-215">V seznamu aplikace hello vyberte **Envoy**.</span><span class="sxs-lookup"><span data-stu-id="5922a-215">In hello applications list, select **Envoy**.</span></span>

    ![Hello Envoy odkaz v seznamu aplikace hello](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_app.png)  

3. <span data-ttu-id="5922a-217">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5922a-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="5922a-219">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5922a-219">Click **Add** button.</span></span> <span data-ttu-id="5922a-220">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5922a-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="5922a-222">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5922a-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5922a-223">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5922a-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5922a-224">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5922a-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5922a-225">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5922a-225">Test single sign-on</span></span>

<span data-ttu-id="5922a-226">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5922a-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5922a-227">Když kliknete na dlaždici Envoy hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Envoy aplikace.</span><span class="sxs-lookup"><span data-stu-id="5922a-227">When you click hello Envoy tile in hello Access Panel, you should get automatically signed-on tooyour Envoy application.</span></span>
<span data-ttu-id="5922a-228">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5922a-228">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5922a-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5922a-229">Additional resources</span></span>

* [<span data-ttu-id="5922a-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5922a-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5922a-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5922a-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_203.png

