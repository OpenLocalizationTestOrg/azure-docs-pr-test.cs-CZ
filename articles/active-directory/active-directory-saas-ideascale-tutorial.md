---
title: 'Kurz: Azure Active Directory integrace s IdeaScale | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a IdeaScale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 10722b137e7565ee165e73994fd5a60b994719bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a><span data-ttu-id="b8c15-103">Kurz: Azure Active Directory integrace s IdeaScale</span><span class="sxs-lookup"><span data-stu-id="b8c15-103">Tutorial: Azure Active Directory integration with IdeaScale</span></span>

<span data-ttu-id="b8c15-104">V tomto kurzu zjistíte, jak toointegrate IdeaScale s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b8c15-104">In this tutorial, you learn how toointegrate IdeaScale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b8c15-105">Integrace IdeaScale s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b8c15-105">Integrating IdeaScale with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b8c15-106">Můžete řídit ve službě Azure AD, který má přístup tooIdeaScale</span><span class="sxs-lookup"><span data-stu-id="b8c15-106">You can control in Azure AD who has access tooIdeaScale</span></span>
- <span data-ttu-id="b8c15-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIdeaScale (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8c15-107">You can enable your users tooautomatically get signed-on tooIdeaScale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b8c15-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b8c15-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b8c15-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b8c15-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8c15-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8c15-110">Prerequisites</span></span>

<span data-ttu-id="b8c15-111">Integrace služby Azure AD s IdeaScale tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b8c15-111">tooconfigure Azure AD integration with IdeaScale, you need hello following items:</span></span>

- <span data-ttu-id="b8c15-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8c15-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b8c15-113">IdeaScale jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b8c15-113">An IdeaScale single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b8c15-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8c15-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b8c15-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b8c15-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b8c15-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b8c15-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b8c15-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b8c15-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b8c15-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b8c15-118">Scenario description</span></span>
<span data-ttu-id="b8c15-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8c15-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b8c15-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b8c15-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b8c15-121">Přidání IdeaScale z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b8c15-121">Adding IdeaScale from hello gallery</span></span>
2. <span data-ttu-id="b8c15-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8c15-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ideascale-from-hello-gallery"></a><span data-ttu-id="b8c15-123">Přidání IdeaScale z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b8c15-123">Adding IdeaScale from hello gallery</span></span>
<span data-ttu-id="b8c15-124">tooconfigure hello integrace IdeaScale do Azure AD, je nutné tooadd IdeaScale hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b8c15-124">tooconfigure hello integration of IdeaScale into Azure AD, you need tooadd IdeaScale from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b8c15-125">**tooadd IdeaScale z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b8c15-125">**tooadd IdeaScale from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b8c15-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b8c15-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b8c15-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b8c15-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b8c15-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8c15-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b8c15-133">Hello vyhledávacího pole zadejte **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-133">In hello search box, type **IdeaScale**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. <span data-ttu-id="b8c15-135">Na panelu výsledků hello vyberte **IdeaScale**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8c15-135">In hello results panel, select **IdeaScale**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b8c15-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8c15-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b8c15-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s IdeaScale podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b8c15-138">In this section, you configure and test Azure AD single sign-on with IdeaScale based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b8c15-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v IdeaScale je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b8c15-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in IdeaScale is tooa user in Azure AD.</span></span> <span data-ttu-id="b8c15-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v IdeaScale musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b8c15-140">In other words, a link relationship between an Azure AD user and hello related user in IdeaScale needs toobe established.</span></span>

<span data-ttu-id="b8c15-141">V IdeaScale, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b8c15-141">In IdeaScale, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b8c15-142">tooconfigure a testu Azure AD jednotné přihlašování s IdeaScale, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b8c15-142">tooconfigure and test Azure AD single sign-on with IdeaScale, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b8c15-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b8c15-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b8c15-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b8c15-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b8c15-145">**[Vytváření testovacího uživatele IdeaScale](#creating-an-ideascale-test-user)**  -toohave protějšek Britta Simon v IdeaScale, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b8c15-145">**[Creating an IdeaScale test user](#creating-an-ideascale-test-user)** - toohave a counterpart of Britta Simon in IdeaScale that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b8c15-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b8c15-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b8c15-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b8c15-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b8c15-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8c15-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b8c15-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci IdeaScale.</span><span class="sxs-lookup"><span data-stu-id="b8c15-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your IdeaScale application.</span></span>

<span data-ttu-id="b8c15-150">**tooconfigure Azure AD jednotné přihlašování s IdeaScale, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b8c15-150">**tooconfigure Azure AD single sign-on with IdeaScale, perform hello following steps:**</span></span>

1. <span data-ttu-id="b8c15-151">V portálu Azure, na hello hello **IdeaScale** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-151">In hello Azure portal, on hello **IdeaScale** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b8c15-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b8c15-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. <span data-ttu-id="b8c15-155">Na hello **IdeaScale domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b8c15-155">On hello **IdeaScale Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    <span data-ttu-id="b8c15-157">a.</span><span class="sxs-lookup"><span data-stu-id="b8c15-157">a.</span></span> <span data-ttu-id="b8c15-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.ideascale.com`</span><span class="sxs-lookup"><span data-stu-id="b8c15-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.ideascale.com`</span></span>

    <span data-ttu-id="b8c15-159">b.</span><span class="sxs-lookup"><span data-stu-id="b8c15-159">b.</span></span> <span data-ttu-id="b8c15-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="b8c15-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > <span data-ttu-id="b8c15-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b8c15-161">These values are not real.</span></span> <span data-ttu-id="b8c15-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b8c15-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b8c15-163">Obraťte se na [tým podpory IdeaScale klienta](http://support.ideascale.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b8c15-163">Contact [IdeaScale Client support team](http://support.ideascale.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="b8c15-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b8c15-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. <span data-ttu-id="b8c15-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8c15-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b8c15-168">Na hello **IdeaScale konfigurace** klikněte na tlačítko **konfigurace IdeaScale** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b8c15-168">On hello **IdeaScale Configuration** section, click **Configure IdeaScale** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b8c15-169">Kopírování hello **Sign-Out adresy URL a SAML Entity ID** z hello **Stručná referenční příručka části**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-169">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. <span data-ttu-id="b8c15-171">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti IdeaScale tooyour.</span><span class="sxs-lookup"><span data-stu-id="b8c15-171">In a different web browser window, log in tooyour IdeaScale company site as an administrator.</span></span>

8. <span data-ttu-id="b8c15-172">Přejděte příliš**komunity nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-172">Go too**Community Settings**.</span></span>
   
    <span data-ttu-id="b8c15-173">![Nastavení komunity](./media/active-directory-saas-ideascale-tutorial/ic790847.png "nastavení komunity")</span><span class="sxs-lookup"><span data-stu-id="b8c15-173">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

9. <span data-ttu-id="b8c15-174">Přejděte příliš**zabezpečení \> nastavení jednotného Sign-on**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-174">Go too**Security \> Single Signon Settings**.</span></span>
   
    <span data-ttu-id="b8c15-175">![Jednoduchého nastavení Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790848.png "jednoduchého nastavení Sign-on")</span><span class="sxs-lookup"><span data-stu-id="b8c15-175">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790848.png "Single Signon Settings")</span></span>

10. <span data-ttu-id="b8c15-176">Jako **typu Single-Sign-on**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-176">As **Single-Signon Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="b8c15-177">![Jeden typ Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790849.png "jeden typ Sign-on")</span><span class="sxs-lookup"><span data-stu-id="b8c15-177">![Single Signon Type](./media/active-directory-saas-ideascale-tutorial/ic790849.png "Single Signon Type")</span></span>

11. <span data-ttu-id="b8c15-178">Na hello **nastavení jednotného Sign-on** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b8c15-178">On hello **Single Signon Settings** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="b8c15-179">![Jednoduchého nastavení Sign-on](./media/active-directory-saas-ideascale-tutorial/ic790850.png "jednoduchého nastavení Sign-on")</span><span class="sxs-lookup"><span data-stu-id="b8c15-179">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790850.png "Single Signon Settings")</span></span>
   
    <span data-ttu-id="b8c15-180">a.</span><span class="sxs-lookup"><span data-stu-id="b8c15-180">a.</span></span> <span data-ttu-id="b8c15-181">V **SAML IdP Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8c15-181">In **SAML IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b8c15-182">b.</span><span class="sxs-lookup"><span data-stu-id="b8c15-182">b.</span></span> <span data-ttu-id="b8c15-183">Kopírovat obsah hello souboru metadat stažený z portálu Azure a vložte jej do hello **SAML IdP Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b8c15-183">Copy hello content of your downloaded metadata file from Azure portal, and paste it into hello **SAML IdP Metadata** textbox.</span></span>

    <span data-ttu-id="b8c15-184">c.</span><span class="sxs-lookup"><span data-stu-id="b8c15-184">c.</span></span> <span data-ttu-id="b8c15-185">V **adresy URL odhlašovací úspěch** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8c15-185">In **Logout Success URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b8c15-186">d.</span><span class="sxs-lookup"><span data-stu-id="b8c15-186">d.</span></span> <span data-ttu-id="b8c15-187">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-187">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="b8c15-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b8c15-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b8c15-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b8c15-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b8c15-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b8c15-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b8c15-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b8c15-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="b8c15-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b8c15-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b8c15-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b8c15-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b8c15-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b8c15-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b8c15-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b8c15-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b8c15-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b8c15-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b8c15-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b8c15-203">a.</span><span class="sxs-lookup"><span data-stu-id="b8c15-203">a.</span></span> <span data-ttu-id="b8c15-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b8c15-205">b.</span><span class="sxs-lookup"><span data-stu-id="b8c15-205">b.</span></span> <span data-ttu-id="b8c15-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b8c15-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b8c15-207">c.</span><span class="sxs-lookup"><span data-stu-id="b8c15-207">c.</span></span> <span data-ttu-id="b8c15-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b8c15-209">d.</span><span class="sxs-lookup"><span data-stu-id="b8c15-209">d.</span></span> <span data-ttu-id="b8c15-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-210">Click **Create**.</span></span>
 
### <a name="creating-an-ideascale-test-user"></a><span data-ttu-id="b8c15-211">Vytváření testovacího uživatele IdeaScale</span><span class="sxs-lookup"><span data-stu-id="b8c15-211">Creating an IdeaScale test user</span></span>

<span data-ttu-id="b8c15-212">Uživatelé toolog tooenable Azure AD do IdeaScale, se musí být zřízená v tooIdeaScale.</span><span class="sxs-lookup"><span data-stu-id="b8c15-212">tooenable Azure AD users toolog into IdeaScale, they must be provisioned in tooIdeaScale.</span></span> <span data-ttu-id="b8c15-213">V případě hello IdeaScale zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b8c15-213">In hello case of IdeaScale, provisioning is a manual task.</span></span>

<span data-ttu-id="b8c15-214">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b8c15-214">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="b8c15-215">Přihlaste se tooyour **IdeaScale** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b8c15-215">Log in tooyour **IdeaScale** company site as administrator.</span></span>

2. <span data-ttu-id="b8c15-216">Přejděte příliš**komunity nastavení**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-216">Go too**Community Settings**.</span></span>
   
    <span data-ttu-id="b8c15-217">![Nastavení komunity](./media/active-directory-saas-ideascale-tutorial/ic790847.png "nastavení komunity")</span><span class="sxs-lookup"><span data-stu-id="b8c15-217">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

3. <span data-ttu-id="b8c15-218">Přejděte příliš**základní nastavení \> člen správu**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-218">Go too**Basic Settings \> Member Management**.</span></span>

4. <span data-ttu-id="b8c15-219">Klikněte na tlačítko **přidat člena**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-219">Click **Add Member**.</span></span>
   
    <span data-ttu-id="b8c15-220">![Člen správu](./media/active-directory-saas-ideascale-tutorial/ic790852.png "člen správy")</span><span class="sxs-lookup"><span data-stu-id="b8c15-220">![Member Management](./media/active-directory-saas-ideascale-tutorial/ic790852.png "Member Management")</span></span>

5. <span data-ttu-id="b8c15-221">V části Přidání nového člena hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b8c15-221">In hello Add New Member section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b8c15-222">![Přidání nového člena](./media/active-directory-saas-ideascale-tutorial/ic790853.png "přidání nového člena")</span><span class="sxs-lookup"><span data-stu-id="b8c15-222">![Add New Member](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Add New Member")</span></span>
   
    <span data-ttu-id="b8c15-223">a.</span><span class="sxs-lookup"><span data-stu-id="b8c15-223">a.</span></span> <span data-ttu-id="b8c15-224">V hello **e-mailové adresy** textovému poli, typ hello e-mailovou adresu chcete tooprovision platného účtu AAD.</span><span class="sxs-lookup"><span data-stu-id="b8c15-224">In hello **Email Addresses** textbox, type hello email address of a valid AAD account you want tooprovision.</span></span>
   
    <span data-ttu-id="b8c15-225">b.</span><span class="sxs-lookup"><span data-stu-id="b8c15-225">b.</span></span> <span data-ttu-id="b8c15-226">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-226">Click **Save Changes**.</span></span> 
   
    >[!NOTE]
    ><span data-ttu-id="b8c15-227">Držitel účtu Azure Active Directory Hello získá e-mail s účtem odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="b8c15-227">hello Azure Active Directory account holder gets an email with a link tooconfirm hello account before it becomes active.</span></span>
      
>[!NOTE]
><span data-ttu-id="b8c15-228">Můžete použít všechny ostatní IdeaScale uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované IdeaScale tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b8c15-228">You can use any other IdeaScale user account creation tools or APIs provided by IdeaScale tooprovision AAD user accounts.</span></span>
 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b8c15-229">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b8c15-229">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b8c15-230">V této části povolíte tak, že udělíte přístup tooIdeaScale toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b8c15-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIdeaScale.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b8c15-232">**tooassign Britta Simon tooIdeaScale, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b8c15-232">**tooassign Britta Simon tooIdeaScale, perform hello following steps:**</span></span>

1. <span data-ttu-id="b8c15-233">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b8c15-235">V seznamu aplikace hello vyberte **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-235">In hello applications list, select **IdeaScale**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. <span data-ttu-id="b8c15-237">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b8c15-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b8c15-239">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b8c15-239">Click **Add** button.</span></span> <span data-ttu-id="b8c15-240">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8c15-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b8c15-242">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b8c15-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b8c15-243">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8c15-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b8c15-244">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b8c15-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b8c15-245">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b8c15-245">Testing single sign-on</span></span>


<span data-ttu-id="b8c15-246">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b8c15-246">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b8c15-247">Když kliknete na dlaždici IdeaScale hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour IdeaScale aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8c15-247">When you click hello IdeaScale tile in hello Access Panel, you should get automatically signed-on tooyour IdeaScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8c15-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b8c15-248">Additional resources</span></span>

* [<span data-ttu-id="b8c15-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b8c15-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b8c15-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b8c15-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

