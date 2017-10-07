---
title: 'Kurz: Azure Active Directory integrace s YouEarnedIt | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a YouEarnedIt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3011d44d-dfcf-4061-888f-cff90fbc8150
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: cc9a8ae2f92751cf3fadbeec23c8319c83728a33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a><span data-ttu-id="13b32-103">Kurz: Azure Active Directory integrace s YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="13b32-103">Tutorial: Azure Active Directory integration with YouEarnedIt</span></span>

<span data-ttu-id="13b32-104">V tomto kurzu zjistíte, jak toointegrate YouEarnedIt s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="13b32-104">In this tutorial, you learn how toointegrate YouEarnedIt with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="13b32-105">Integrace YouEarnedIt s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="13b32-105">Integrating YouEarnedIt with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="13b32-106">Můžete ovládat ve službě Azure AD, který má přístup tooYouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="13b32-106">You can control in Azure AD who has access tooYouEarnedIt.</span></span>
- <span data-ttu-id="13b32-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooYouEarnedIt (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="13b32-107">You can enable your users tooautomatically get signed-on tooYouEarnedIt (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="13b32-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="13b32-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="13b32-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="13b32-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13b32-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="13b32-110">Prerequisites</span></span>

<span data-ttu-id="13b32-111">Integrace služby Azure AD s YouEarnedIt tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="13b32-111">tooconfigure Azure AD integration with YouEarnedIt, you need hello following items:</span></span>

- <span data-ttu-id="13b32-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="13b32-112">An Azure AD subscription</span></span>
- <span data-ttu-id="13b32-113">YouEarnedIt jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="13b32-113">A YouEarnedIt single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="13b32-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="13b32-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="13b32-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="13b32-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="13b32-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="13b32-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="13b32-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="13b32-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="13b32-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="13b32-118">Scenario description</span></span>
<span data-ttu-id="13b32-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="13b32-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="13b32-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="13b32-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="13b32-121">Přidání YouEarnedIt z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="13b32-121">Adding YouEarnedIt from hello gallery</span></span>
2. <span data-ttu-id="13b32-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="13b32-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-youearnedit-from-hello-gallery"></a><span data-ttu-id="13b32-123">Přidání YouEarnedIt z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="13b32-123">Adding YouEarnedIt from hello gallery</span></span>
<span data-ttu-id="13b32-124">tooconfigure hello integrace YouEarnedIt do Azure AD, je nutné tooadd YouEarnedIt hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="13b32-124">tooconfigure hello integration of YouEarnedIt into Azure AD, you need tooadd YouEarnedIt from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="13b32-125">**tooadd YouEarnedIt z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13b32-125">**tooadd YouEarnedIt from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="13b32-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="13b32-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="13b32-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="13b32-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="13b32-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="13b32-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="13b32-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13b32-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="13b32-133">Hello vyhledávacího pole zadejte **YouEarnedt**, vyberte **YouEarnedt** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="13b32-133">In hello search box, type **YouEarnedt**, select  **YouEarnedt**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![YouEarnedIt v seznamu výsledků hello](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="13b32-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="13b32-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="13b32-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s YouEarnedIt podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="13b32-136">In this section, you configure and test Azure AD single sign-on with YouEarnedIt based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="13b32-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v YouEarnedIt je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="13b32-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in YouEarnedIt is tooa user in Azure AD.</span></span> <span data-ttu-id="13b32-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v YouEarnedIt musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="13b32-138">In other words, a link relationship between an Azure AD user and hello related user in YouEarnedIt needs toobe established.</span></span>

<span data-ttu-id="13b32-139">V YouEarnedIt, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="13b32-139">In YouEarnedIt, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="13b32-140">tooconfigure a testu Azure AD jednotné přihlašování s YouEarnedIt, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="13b32-140">tooconfigure and test Azure AD single sign-on with YouEarnedIt, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="13b32-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="13b32-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="13b32-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="13b32-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="13b32-143">**[Vytvoření zkušebního uživatele YouEarnedIt](#create-a-youearnedit-test-user)**  -toohave protějšek Britta Simon v YouEarnedIt, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="13b32-143">**[Create a YouEarnedIt test user](#create-a-youearnedit-test-user)** - toohave a counterpart of Britta Simon in YouEarnedIt that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="13b32-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="13b32-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="13b32-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="13b32-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="13b32-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="13b32-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="13b32-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci YouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="13b32-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your YouEarnedIt application.</span></span>

<span data-ttu-id="13b32-148">**tooconfigure Azure AD jednotné přihlašování s YouEarnedIt, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13b32-148">**tooconfigure Azure AD single sign-on with YouEarnedIt, perform hello following steps:**</span></span>

1. <span data-ttu-id="13b32-149">V portálu Azure, na hello hello **YouEarnedIt** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="13b32-149">In hello Azure portal, on hello **YouEarnedIt** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="13b32-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="13b32-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_samlbase.png)

3. <span data-ttu-id="13b32-153">Na hello **YouEarnedIt domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="13b32-153">On hello **YouEarnedIt Domain and URLs** section, perform hello following steps:</span></span>

    ![YouEarnedIt domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_url.png)

    <span data-ttu-id="13b32-155">a.</span><span class="sxs-lookup"><span data-stu-id="13b32-155">a.</span></span> <span data-ttu-id="13b32-156">V hello **přihlašovací adresa URL** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="13b32-156">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    | <span data-ttu-id="13b32-157">Prostředí</span><span class="sxs-lookup"><span data-stu-id="13b32-157">Environment</span></span>  | <span data-ttu-id="13b32-158">vzor</span><span class="sxs-lookup"><span data-stu-id="13b32-158">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="13b32-159">Produkční</span><span class="sxs-lookup"><span data-stu-id="13b32-159">Production</span></span> | `https://<company name>.youearnedit.com/users/sign_in` |
    | <span data-ttu-id="13b32-160">Izolovaný prostor</span><span class="sxs-lookup"><span data-stu-id="13b32-160">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    <span data-ttu-id="13b32-161">b.</span><span class="sxs-lookup"><span data-stu-id="13b32-161">b.</span></span> <span data-ttu-id="13b32-162">V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="13b32-162">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>
    | <span data-ttu-id="13b32-163">Prostředí</span><span class="sxs-lookup"><span data-stu-id="13b32-163">Environment</span></span>  | <span data-ttu-id="13b32-164">vzor</span><span class="sxs-lookup"><span data-stu-id="13b32-164">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="13b32-165">Produkční</span><span class="sxs-lookup"><span data-stu-id="13b32-165">Production</span></span> | `https://<company name>.youearnedit.com` |
    | <span data-ttu-id="13b32-166">Izolovaný prostor</span><span class="sxs-lookup"><span data-stu-id="13b32-166">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com` |

    > [!NOTE] 
    > <span data-ttu-id="13b32-167">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="13b32-167">These values are not real.</span></span> <span data-ttu-id="13b32-168">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="13b32-168">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="13b32-169">Obraťte se na [tým podpory YouEarnedIt klienta](https://youearnedit.freshdesk.com/support/tickets/new) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="13b32-169">Contact [YouEarnedIt Client support team](https://youearnedit.freshdesk.com/support/tickets/new) tooget these values.</span></span> 
 
4. <span data-ttu-id="13b32-170">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="13b32-170">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_certificate.png) 

5. <span data-ttu-id="13b32-172">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13b32-172">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-youearnedit-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="13b32-174">Na hello **YouEarnedIt konfigurace** klikněte na tlačítko **konfigurace YouEarnedIt** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="13b32-174">On hello **YouEarnedIt Configuration** section, click **Configure YouEarnedIt** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="13b32-175">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="13b32-175">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace YouEarnedIt](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_configure.png) 

7. <span data-ttu-id="13b32-177">tooconfigure jednotného přihlašování na **YouEarnedIt** straně, je nutné stáhnout hello toosend **Certificate(Base64)** a **SAML jeden přihlašování adresa URL služby** příliš[Tým podpory YouEarnedIt](https://youearnedit.freshdesk.com/support/tickets/new).</span><span class="sxs-lookup"><span data-stu-id="13b32-177">tooconfigure single sign-on on **YouEarnedIt** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[YouEarnedIt support team](https://youearnedit.freshdesk.com/support/tickets/new).</span></span> <span data-ttu-id="13b32-178">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="13b32-178">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="13b32-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="13b32-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="13b32-180">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="13b32-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="13b32-181">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="13b32-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="13b32-182">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="13b32-182">Create an Azure AD test user</span></span>

<span data-ttu-id="13b32-183">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="13b32-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="13b32-185">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13b32-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="13b32-186">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13b32-186">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="13b32-188">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="13b32-188">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="13b32-190">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13b32-190">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="13b32-192">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="13b32-192">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="13b32-194">a.</span><span class="sxs-lookup"><span data-stu-id="13b32-194">a.</span></span> <span data-ttu-id="13b32-195">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="13b32-195">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="13b32-196">b.</span><span class="sxs-lookup"><span data-stu-id="13b32-196">b.</span></span> <span data-ttu-id="13b32-197">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="13b32-197">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="13b32-198">c.</span><span class="sxs-lookup"><span data-stu-id="13b32-198">c.</span></span> <span data-ttu-id="13b32-199">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="13b32-199">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="13b32-200">d.</span><span class="sxs-lookup"><span data-stu-id="13b32-200">d.</span></span> <span data-ttu-id="13b32-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="13b32-201">Click **Create**.</span></span>
 
### <a name="create-a-youearnedit-test-user"></a><span data-ttu-id="13b32-202">Vytvoření zkušebního uživatele YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="13b32-202">Create a YouEarnedIt test user</span></span>

<span data-ttu-id="13b32-203">V této části vytvoříte volal Britta Simon v YouEarnedIt uživatele.</span><span class="sxs-lookup"><span data-stu-id="13b32-203">In this section, you create a user called Britta Simon in YouEarnedIt.</span></span> <span data-ttu-id="13b32-204">Spojte se s YouEarnedIt podporu team tooadd hello uživateli v platformě YouEarnedIt hello.</span><span class="sxs-lookup"><span data-stu-id="13b32-204">Please work with YouEarnedIt support team tooadd hello users in hello YouEarnedIt platform.</span></span>

>[!NOTE]
><span data-ttu-id="13b32-205">YouEarnedIt očekávat hello zprostředkovatele Identity toosupply EmailAddress nebo uživatelské jméno v atributu NameID hello.</span><span class="sxs-lookup"><span data-stu-id="13b32-205">YouEarnedIt expect hello Identity Provider toosupply an EmailAddress  or UserName in hello NameID attribute.</span></span> <span data-ttu-id="13b32-206">Ověřování se nezdaří, pokud odpovídající uživatelské jméno nebo EmailAddress nebyl nalezen v databázi hello nebo neodpovídá přesně.</span><span class="sxs-lookup"><span data-stu-id="13b32-206">Authentication will fail if a corresponding UserName or EmailAddress is not found within hello database or does not match exactly.</span></span> <span data-ttu-id="13b32-207">To bude vyžadovat, že účty naimportovat do systému YouEarnedIt hello před integrací hello jednotné přihlašování, (obvykle buď prostřednictvím rozhraní API nebo CSV import).</span><span class="sxs-lookup"><span data-stu-id="13b32-207">This will require that accounts be imported into hello YouEarnedIt system before hello SSO integration (Typically either via API or CSV import).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="13b32-208">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="13b32-208">Assign hello Azure AD test user</span></span>

<span data-ttu-id="13b32-209">V této části povolíte tak, že udělíte přístup tooYouEarnedIt toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="13b32-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYouEarnedIt.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="13b32-211">**tooassign Britta Simon tooYouEarnedIt, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13b32-211">**tooassign Britta Simon tooYouEarnedIt, perform hello following steps:**</span></span>

1. <span data-ttu-id="13b32-212">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="13b32-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="13b32-214">V seznamu aplikace hello vyberte **YouEarnedIt**.</span><span class="sxs-lookup"><span data-stu-id="13b32-214">In hello applications list, select **YouEarnedIt**.</span></span>

    ![v seznamu aplikace hello Hello YouEarnedIt odkaz](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_app.png)  

3. <span data-ttu-id="13b32-216">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="13b32-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="13b32-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13b32-218">Click **Add** button.</span></span> <span data-ttu-id="13b32-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13b32-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="13b32-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="13b32-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="13b32-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13b32-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="13b32-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13b32-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="13b32-224">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="13b32-224">Test single sign-on</span></span>

<span data-ttu-id="13b32-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="13b32-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="13b32-226">Když kliknete na dlaždici YouEarnedIt hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour YouEarnedIt aplikace.</span><span class="sxs-lookup"><span data-stu-id="13b32-226">When you click hello YouEarnedIt tile in hello Access Panel, you should get automatically signed-on tooyour YouEarnedIt application.</span></span>
<span data-ttu-id="13b32-227">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="13b32-227">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="13b32-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="13b32-228">Additional resources</span></span>

* [<span data-ttu-id="13b32-229">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="13b32-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="13b32-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="13b32-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png

