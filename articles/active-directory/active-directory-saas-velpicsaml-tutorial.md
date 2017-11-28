---
title: 'Kurz: Azure Active Directory integrace s Velpic SAML | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Velpic SAML."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 613947d8fe95113382a2cdc0f79ce9eda85a0127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a><span data-ttu-id="1aaa4-103">Kurz: Azure Active Directory integrace s Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="1aaa4-103">Tutorial: Azure Active Directory integration with Velpic SAML</span></span>

<span data-ttu-id="1aaa4-104">V tomto kurzu zjistíte, jak toointegrate Velpic SAML s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1aaa4-104">In this tutorial, you learn how toointegrate Velpic SAML with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1aaa4-105">Integrace Velpic SAML s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-105">Integrating Velpic SAML with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1aaa4-106">Můžete řídit ve službě Azure AD, který má přístup tooVelpic SAML</span><span class="sxs-lookup"><span data-stu-id="1aaa4-106">You can control in Azure AD who has access tooVelpic SAML</span></span>
- <span data-ttu-id="1aaa4-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooVelpic SAML (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1aaa4-107">You can enable your users tooautomatically get signed-on tooVelpic SAML (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1aaa4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="1aaa4-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="1aaa4-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1aaa4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1aaa4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1aaa4-110">Prerequisites</span></span>

<span data-ttu-id="1aaa4-111">tooconfigure integrace Azure AD s Velpic SAML, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-111">tooconfigure Azure AD integration with Velpic SAML, you need hello following items:</span></span>

- <span data-ttu-id="1aaa4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1aaa4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1aaa4-113">Velpic SAML jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1aaa4-113">A Velpic SAML single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1aaa4-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1aaa4-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1aaa4-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="1aaa4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1aaa4-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1aaa4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1aaa4-118">Scenario description</span></span>
<span data-ttu-id="1aaa4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1aaa4-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1aaa4-121">Přidání Velpic SAML z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1aaa4-121">Adding Velpic SAML from hello gallery</span></span>
2. <span data-ttu-id="1aaa4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1aaa4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-velpic-saml-from-hello-gallery"></a><span data-ttu-id="1aaa4-123">Přidání Velpic SAML z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1aaa4-123">Adding Velpic SAML from hello gallery</span></span>
<span data-ttu-id="1aaa4-124">tooconfigure hello integrace Velpic SAML do Azure AD, je nutné tooadd Velpic SAML hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-124">tooconfigure hello integration of Velpic SAML into Azure AD, you need tooadd Velpic SAML from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1aaa4-125">**tooadd Velpic SAML z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1aaa4-125">**tooadd Velpic SAML from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1aaa4-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1aaa4-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1aaa4-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1aaa4-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1aaa4-133">Hello vyhledávacího pole zadejte **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-133">In hello search box, type **Velpic SAML**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. <span data-ttu-id="1aaa4-135">Na panelu výsledků hello vyberte **Velpic SAML**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-135">In hello results panel, select **Velpic SAML**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1aaa4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1aaa4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1aaa4-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Velpic SAML podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1aaa4-138">In this section, you configure and test Azure AD single sign-on with Velpic SAML based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1aaa4-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Velpic SAML je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Velpic SAML is tooa user in Azure AD.</span></span> <span data-ttu-id="1aaa4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Velpic SAML musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-140">In other words, a link relationship between an Azure AD user and hello related user in Velpic SAML needs toobe established.</span></span>

<span data-ttu-id="1aaa4-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Velpic SAML.</span></span>

<span data-ttu-id="1aaa4-142">tooconfigure a testu Azure AD jednotné přihlašování s Velpic SAML, budete potřebovat následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-142">tooconfigure and test Azure AD single sign-on with Velpic SAML, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1aaa4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1aaa4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1aaa4-145">**[Vytvoření zkušebního uživatele Velpic SAML](#creating-a-velpic-saml-test-user)**  -toohave protějšek Britta Simon v Velpic SAML, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-145">**[Creating a Velpic SAML test user](#creating-a-velpic-saml-test-user)** - toohave a counterpart of Britta Simon in Velpic SAML that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="1aaa4-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1aaa4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1aaa4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1aaa4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1aaa4-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Velpic SAML application.</span></span>

<span data-ttu-id="1aaa4-150">**tooconfigure Azure AD jednotné přihlašování s Velpic SAML, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1aaa4-150">**tooconfigure Azure AD single sign-on with Velpic SAML, perform hello following steps:**</span></span>

1. <span data-ttu-id="1aaa4-151">V hello Azure Management portal na hello **Velpic SAML** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-151">In hello Azure Management portal, on hello **Velpic SAML** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1aaa4-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. <span data-ttu-id="1aaa4-155">Zadejte podrobnosti hello v hello **domény Velpic SAML a adres URL** části -</span><span class="sxs-lookup"><span data-stu-id="1aaa4-155">Enter hello details in hello **Velpic SAML Domain and URLs** section-</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    <span data-ttu-id="1aaa4-157">a.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-157">a.</span></span> <span data-ttu-id="1aaa4-158">V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://<sub-domain>.velpicsaml.net`</span><span class="sxs-lookup"><span data-stu-id="1aaa4-158">In hello **Sign-on URL** textbox, type hello value as: `https://<sub-domain>.velpicsaml.net`</span></span>

    <span data-ttu-id="1aaa4-159">b.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-159">b.</span></span> <span data-ttu-id="1aaa4-160">V hello **identifikátor** textovému poli, vložte hello **'jednotné přihlašování na adresu URL,** hodnota`https://auth.velpic.com/saml/v2/<entity-id>/login`</span><span class="sxs-lookup"><span data-stu-id="1aaa4-160">In hello **Identifier** textbox, paste hello **‘Single sign on URL’** value `https://auth.velpic.com/saml/v2/<entity-id>/login`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="1aaa4-161">Upozorňujeme, že hello adresa URL přihlašování budou poskytovat hello team Velpic SAML a hodnotu identifikátoru bude k dispozici, když konfigurujete hello jednotného přihlašování k modulu plug-in na straně Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-161">Please note that hello Sign on URL will be provided by hello Velpic SAML team and Identifier value will be available when you configure hello SSO Plugin on Velpic SAML side.</span></span> <span data-ttu-id="1aaa4-162">Je nutné toocopy, který ze stránky aplikace Velpic SAML a vložte ji sem.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-162">You need toocopy that value from Velpic SAML application  page and paste it here.</span></span>

4. <span data-ttu-id="1aaa4-163">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. <span data-ttu-id="1aaa4-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1aaa4-167">Na hello Velpic SAML konfigurační oddíl klikněte na konfigurace SAML Velpic tooopen konfigurovat přihlašování okno.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-167">On hello Velpic SAML Configuration section, click Configure Velpic SAML tooopen Configure sign-on window.</span></span> <span data-ttu-id="1aaa4-168">Zkopírujte hello SAML Entity ID z hello Stručná referenční příručka části.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-168">Copy hello SAML Entity ID from hello Quick Reference section.</span></span>

7. <span data-ttu-id="1aaa4-169">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-169">In a different web browser window, log into your Velpic SAML company site as an administrator.</span></span>

8. <span data-ttu-id="1aaa4-170">Klikněte na **spravovat** kartě a přejděte příliš**integrace** část, kde je nutné tooclick na **modulů plug-in** tlačítko toocreate nový modul plug-in pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-170">Click on **Manage** tab and go too**Integration** section where you need tooclick on **Plugins** button toocreate new plugin for Sign-In.</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. <span data-ttu-id="1aaa4-172">Klikněte na hello **'Přidání modulu plug-in'** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-172">Click on hello **‘Add plugin’** button.</span></span>
    
    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. <span data-ttu-id="1aaa4-174">Klikněte na hello **SAML** dlaždici stránce Přidat modul plug-in hello.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-174">Click on hello **SAML** tile in hello Add Plugin page.</span></span>
    
    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. <span data-ttu-id="1aaa4-176">Zadejte název hello hello nové zásuvný modul SAML a klikněte na tlačítko hello **"Přidat"** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-176">Enter hello name of hello new SAML plugin and click hello **‘Add’** button.</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. <span data-ttu-id="1aaa4-178">Zadejte podrobnosti hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-178">Enter hello details as follows:</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    <span data-ttu-id="1aaa4-180">a.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-180">a.</span></span> <span data-ttu-id="1aaa4-181">V hello **název** textovému poli, název typu hello zásuvný modul SAML.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-181">In hello **Name** textbox, type hello name of SAML plugin.</span></span>

    <span data-ttu-id="1aaa4-182">b.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-182">b.</span></span> <span data-ttu-id="1aaa4-183">V hello **URL vystavitele** textovému poli, vložte hello **SAML Entity ID** jste zkopírovali ze hello **konfigurovat přihlášení** okno hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-183">In hello **Issuer URL** textbox, paste hello **SAML Entity ID** you copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

    <span data-ttu-id="1aaa4-184">c.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-184">c.</span></span> <span data-ttu-id="1aaa4-185">V hello **zprostředkovatele metadat konfigurace** nahrát hello soubor XML s metadaty souboru, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-185">In hello **Provider Metadata Config** upload hello Metadata XML file which you downloaded from Azure portal.</span></span>

    <span data-ttu-id="1aaa4-186">d.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-186">d.</span></span> <span data-ttu-id="1aaa4-187">Můžete také tooenable SAML jenom při zřizování čas povolením hello **, automaticky vytvoří nové uživatele'** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-187">You can also choose tooenable SAML just in time provisioning by enabling hello **‘Auto create new users’** checkbox.</span></span> <span data-ttu-id="1aaa4-188">Pokud uživatel neexistuje v Velpic a tento příznak není povolen, hello přihlášení z Azure se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-188">If a user doesn’t exist in Velpic and this flag is not enabled, hello login from Azure will fail.</span></span> <span data-ttu-id="1aaa4-189">Pokud příznak hello je povoleno hello uživatele budou automaticky zřídí do Velpic během hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-189">If hello flag is enabled hello user will automatically be provisioned into Velpic at hello time of login.</span></span> 

    <span data-ttu-id="1aaa4-190">e.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-190">e.</span></span> <span data-ttu-id="1aaa4-191">Kopírování hello **jednotné přihlašování na adrese URL** z hello textového pole a vložte jej do hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-191">Copy hello **Single sign on URL** from hello text box and paste it in hello Azure portal.</span></span>
    
    <span data-ttu-id="1aaa4-192">f.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-192">f.</span></span> <span data-ttu-id="1aaa4-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-193">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1aaa4-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1aaa4-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="1aaa4-195">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-195">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1aaa4-197">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1aaa4-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1aaa4-198">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-198">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1aaa4-200">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-200">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1aaa4-202">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-202">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1aaa4-204">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1aaa4-206">a.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-206">a.</span></span> <span data-ttu-id="1aaa4-207">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1aaa4-208">b.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-208">b.</span></span> <span data-ttu-id="1aaa4-209">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1aaa4-210">c.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-210">c.</span></span> <span data-ttu-id="1aaa4-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1aaa4-212">d.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-212">d.</span></span> <span data-ttu-id="1aaa4-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-213">Click **Create**.</span></span>
 
### <a name="creating-a-velpic-saml-test-user"></a><span data-ttu-id="1aaa4-214">Vytvoření zkušebního uživatele Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="1aaa4-214">Creating a Velpic SAML test user</span></span>

<span data-ttu-id="1aaa4-215">Tento krok se obvykle nevyžaduje jako aplikace hello podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-215">This step is usually not required as hello application supports just in time user provisioning.</span></span> <span data-ttu-id="1aaa4-216">Pokud zřizování hello automatické uživatelů není povolena můžete vytvoření ruční uživatele provádí, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-216">If hello automatic user provisioning is not enabled then manual user creation can be done as described below.</span></span>

<span data-ttu-id="1aaa4-217">Přihlaste se k serveru vaší společnosti Velpic SAML jako správce a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1aaa4-217">Log into your Velpic SAML company site as an administrator and perform following steps:</span></span>
    
1. <span data-ttu-id="1aaa4-218">Klikněte na kartu Správa a přejděte tooUsers části potom klikněte na nové tlačítko tooadd uživatele.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-218">Click on Manage tab and go tooUsers section, then click on New button tooadd users.</span></span>

    ![Přidat uživatele](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. <span data-ttu-id="1aaa4-220">Na hello **"Vytvořit nový uživatel"** dialogové okno proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-220">On hello **“Create New User”** dialog page, perform hello following steps.</span></span>

    ![Uživatel](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    <span data-ttu-id="1aaa4-222">a.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-222">a.</span></span> <span data-ttu-id="1aaa4-223">V hello **křestní jméno** textovému poli, typ hello křestní jméno Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-223">In hello **First Name** textbox, type hello first name of Britta Simon.</span></span>

    <span data-ttu-id="1aaa4-224">b.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-224">b.</span></span> <span data-ttu-id="1aaa4-225">V hello **příjmení** textovému poli, typ hello příjmení Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-225">In hello **Last Name** textbox, type hello last name of Britta Simon.</span></span>

    <span data-ttu-id="1aaa4-226">c.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-226">c.</span></span> <span data-ttu-id="1aaa4-227">V hello **uživatelské jméno** textovému poli, typ hello uživatelské jméno Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-227">In hello **User Name** textbox, type hello user name of Britta Simon.</span></span>

    <span data-ttu-id="1aaa4-228">d.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-228">d.</span></span> <span data-ttu-id="1aaa4-229">V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-229">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="1aaa4-230">e.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-230">e.</span></span> <span data-ttu-id="1aaa4-231">Zbývající informace hello je volitelné, že je v případě potřeby můžete vyplnit.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-231">Rest of hello information is optional, you can fill it if needed.</span></span>
    
    <span data-ttu-id="1aaa4-232">f.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-232">f.</span></span> <span data-ttu-id="1aaa4-233">Klikněte na **ULOŽIT**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-233">Click **SAVE**.</span></span>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1aaa4-234">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="1aaa4-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1aaa4-235">V této části povolíte tak, že udělíte svůj přístup tooVelpic SAML Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooVelpic SAML.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1aaa4-237">**tooassign tooVelpic Britta Simon SAML, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1aaa4-237">**tooassign Britta Simon tooVelpic SAML, perform hello following steps:**</span></span>

1. <span data-ttu-id="1aaa4-238">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-238">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1aaa4-240">V seznamu aplikace hello vyberte **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-240">In hello applications list, select **Velpic SAML**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. <span data-ttu-id="1aaa4-242">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1aaa4-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-244">Click **Add** button.</span></span> <span data-ttu-id="1aaa4-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1aaa4-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1aaa4-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1aaa4-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1aaa4-250">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1aaa4-250">Testing single sign-on</span></span>

<span data-ttu-id="1aaa4-251">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-251">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="1aaa4-252">Po kliknutí na tlačítko hello Velpic SAML dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku aplikace Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-252">When you click hello Velpic SAML tile in hello Access Panel, you should get login page of Velpic SAML application.</span></span> <span data-ttu-id="1aaa4-253">Měli byste vidět hello **"přihlásit se přes Azure AD,** na přihlašovací stránku hello tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-253">You should see hello **‘Log In With Azure AD’** button on hello sign in page.</span></span>

    ![Modul plug-in](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. <span data-ttu-id="1aaa4-255">Klikněte na hello **"přihlásit se přes Azure AD,** tlačítko toolog v tooVelpic pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aaa4-255">Click on hello **‘Log In With Azure AD’** button toolog in tooVelpic using your Azure AD account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="1aaa4-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1aaa4-256">Additional resources</span></span>

* [<span data-ttu-id="1aaa4-257">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1aaa4-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1aaa4-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1aaa4-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

