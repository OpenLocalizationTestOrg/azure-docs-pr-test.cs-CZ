---
title: "Kurz: Azure Active Directory integrace s Predictix řazení | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Predictix řazení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2fe2f976-e97f-4368-9695-3e1624409e8b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 0418ef24d7942b6b751c0b4d64e7bd1fba1d6a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a><span data-ttu-id="c1635-103">Kurz: Azure Active Directory integrace s Predictix řazení</span><span class="sxs-lookup"><span data-stu-id="c1635-103">Tutorial: Azure Active Directory integration with Predictix Ordering</span></span>

<span data-ttu-id="c1635-104">V tomto kurzu zjistíte, jak toointegrate Predictix řazení s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c1635-104">In this tutorial, you learn how toointegrate Predictix Ordering with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c1635-105">Integrace Predictix řazení s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c1635-105">Integrating Predictix Ordering with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c1635-106">Můžete řídit ve službě Azure AD, který má přístup tooPredictix řazení.</span><span class="sxs-lookup"><span data-stu-id="c1635-106">You can control in Azure AD who has access tooPredictix Ordering.</span></span>
- <span data-ttu-id="c1635-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPredictix řazení (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1635-107">You can enable your users tooautomatically get signed-on tooPredictix Ordering (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c1635-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c1635-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="c1635-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1635-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1635-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1635-110">Prerequisites</span></span>

<span data-ttu-id="c1635-111">tooconfigure integrace Azure AD s Predictix řazení, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c1635-111">tooconfigure Azure AD integration with Predictix Ordering, you need hello following items:</span></span>

- <span data-ttu-id="c1635-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1635-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c1635-113">Řazení Predictix jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c1635-113">A Predictix Ordering single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1635-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1635-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c1635-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c1635-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c1635-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c1635-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c1635-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1635-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c1635-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c1635-118">Scenario description</span></span>
<span data-ttu-id="c1635-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1635-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c1635-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c1635-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c1635-121">Přidání Predictix řazení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c1635-121">Adding Predictix Ordering from hello gallery</span></span>
2. <span data-ttu-id="c1635-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1635-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-ordering-from-hello-gallery"></a><span data-ttu-id="c1635-123">Přidání Predictix řazení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c1635-123">Adding Predictix Ordering from hello gallery</span></span>
<span data-ttu-id="c1635-124">tooconfigure hello integrace Predictix řazení do Azure AD, je nutné tooadd řazení Predictix hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c1635-124">tooconfigure hello integration of Predictix Ordering into Azure AD, you need tooadd Predictix Ordering from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c1635-125">**tooadd Predictix řazení z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1635-125">**tooadd Predictix Ordering from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1635-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c1635-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="c1635-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c1635-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c1635-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c1635-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="c1635-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1635-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="c1635-133">Hello vyhledávacího pole zadejte **Predictix řazení**, vyberte **Predictix řazení** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1635-133">In hello search box, type **Predictix Ordering**, select **Predictix Ordering** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Predictix řazení v seznamu výsledků hello](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c1635-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1635-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c1635-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Predictix řazení podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c1635-136">In this section, you configure and test Azure AD single sign-on with Predictix Ordering based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c1635-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Predictix řazení je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1635-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Predictix Ordering is tooa user in Azure AD.</span></span> <span data-ttu-id="c1635-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v řazení Predictix musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c1635-138">In other words, a link relationship between an Azure AD user and hello related user in Predictix Ordering needs toobe established.</span></span>

<span data-ttu-id="c1635-139">V Predictix řazení, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c1635-139">In Predictix Ordering, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c1635-140">tooconfigure a testu Azure AD jednotné přihlašování s Predictix řazení, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c1635-140">tooconfigure and test Azure AD single sign-on with Predictix Ordering, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c1635-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c1635-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c1635-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1635-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1635-143">**[Vytvoření zkušebního uživatele Predictix řazení](#create-a-predictix-ordering-test-user)**  -toohave protějšek Britta Simon v Predictix pořadí, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1635-143">**[Create a Predictix Ordering test user](#create-a-predictix-ordering-test-user)** - toohave a counterpart of Britta Simon in Predictix Ordering that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c1635-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c1635-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c1635-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c1635-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c1635-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1635-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c1635-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Predictix řazení.</span><span class="sxs-lookup"><span data-stu-id="c1635-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Predictix Ordering application.</span></span>

<span data-ttu-id="c1635-148">**tooconfigure Azure AD jednotné přihlašování s Predictix řazení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1635-148">**tooconfigure Azure AD single sign-on with Predictix Ordering, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1635-149">V portálu Azure, na hello hello **Predictix řazení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c1635-149">In hello Azure portal, on hello **Predictix Ordering** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="c1635-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c1635-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_samlbase.png)

3. <span data-ttu-id="c1635-153">Na hello **Predictix řazení domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c1635-153">On hello **Predictix Ordering Domain and URLs** section, perform hello following steps:</span></span>

    ![Predictix řazení domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_url.png)

    <span data-ttu-id="c1635-155">a.</span><span class="sxs-lookup"><span data-stu-id="c1635-155">a.</span></span> <span data-ttu-id="c1635-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname-pricing>.ordering.predictix.com/sso/request`</span><span class="sxs-lookup"><span data-stu-id="c1635-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname-pricing>.ordering.predictix.com/sso/request`</span></span>

    <span data-ttu-id="c1635-157">b.</span><span class="sxs-lookup"><span data-stu-id="c1635-157">b.</span></span> <span data-ttu-id="c1635-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="c1635-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span> 
    | |
    |--|
    | `https://<companyname-pricing>.dev.ordering.predictix.com` |
    | `https://<companyname-pricing>.ordering.predictix.com` |

    > [!NOTE] 
    > <span data-ttu-id="c1635-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c1635-159">These values are not real.</span></span> <span data-ttu-id="c1635-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c1635-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c1635-161">Obraťte se na [tým podpory Predictix řazení klienta](https://www.predix.io/support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c1635-161">Contact [Predictix Ordering Client support team](https://www.predix.io/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="c1635-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c1635-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_certificate.png) 

5. <span data-ttu-id="c1635-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1635-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-predictixordering-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c1635-166">Na hello **Predictix řazení konfigurace** klikněte na tlačítko **konfigurovat řazení Predictix** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c1635-166">On hello **Predictix Ordering Configuration** section, click **Configure Predictix Ordering** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c1635-167">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c1635-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Predictix řazení](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_configure.png) 

7. <span data-ttu-id="c1635-169">tooconfigure jednotného přihlašování na **Predictix řazení** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby**  příliš[Predictix řazení tým podpory](https://www.predix.io/support/).</span><span class="sxs-lookup"><span data-stu-id="c1635-169">tooconfigure single sign-on on **Predictix Ordering** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Predictix Ordering support team](https://www.predix.io/support/).</span></span> <span data-ttu-id="c1635-170">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="c1635-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c1635-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c1635-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c1635-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c1635-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c1635-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c1635-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c1635-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1635-174">Create an Azure AD test user</span></span>
<span data-ttu-id="c1635-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c1635-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="c1635-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1635-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1635-178">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1635-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c1635-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c1635-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c1635-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1635-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c1635-184">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c1635-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_04.png)

   <span data-ttu-id="c1635-186">a.</span><span class="sxs-lookup"><span data-stu-id="c1635-186">a.</span></span> <span data-ttu-id="c1635-187">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1635-187">In hello **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="c1635-188">b.</span><span class="sxs-lookup"><span data-stu-id="c1635-188">b.</span></span> <span data-ttu-id="c1635-189">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1635-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

   <span data-ttu-id="c1635-190">c.</span><span class="sxs-lookup"><span data-stu-id="c1635-190">c.</span></span> <span data-ttu-id="c1635-191">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="c1635-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

   <span data-ttu-id="c1635-192">d.</span><span class="sxs-lookup"><span data-stu-id="c1635-192">d.</span></span> <span data-ttu-id="c1635-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c1635-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-ordering-test-user"></a><span data-ttu-id="c1635-194">Vytvoření zkušebního uživatele Predictix řazení</span><span class="sxs-lookup"><span data-stu-id="c1635-194">Create a Predictix Ordering test user</span></span>

<span data-ttu-id="c1635-195">V této části vytvoříte uživatele volal Britta Simon v Predictix řazení.</span><span class="sxs-lookup"><span data-stu-id="c1635-195">In this section, you create a user called Britta Simon in Predictix Ordering.</span></span> <span data-ttu-id="c1635-196">Práce s [Predictix řazení tým podpory](https://www.predix.io/support/) tooadd hello uživatelé v platformě Predictix řazení hello.</span><span class="sxs-lookup"><span data-stu-id="c1635-196">Work with [Predictix Ordering support team](https://www.predix.io/support/) tooadd hello users in hello Predictix Ordering platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c1635-197">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c1635-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c1635-198">V této části můžete povolit Britta Simon toouse Azure jednotné přihlašování a udělení přístupu tooPredictix řazení.</span><span class="sxs-lookup"><span data-stu-id="c1635-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPredictix Ordering.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="c1635-200">**tooassign tooPredictix Britta Simon řazení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1635-200">**tooassign Britta Simon tooPredictix Ordering, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1635-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c1635-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c1635-203">V seznamu aplikace hello vyberte **Predictix řazení**.</span><span class="sxs-lookup"><span data-stu-id="c1635-203">In hello applications list, select **Predictix Ordering**.</span></span>

    ![v seznamu aplikace hello odkaz Predictix řazení Hello](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_app.png)  

3. <span data-ttu-id="c1635-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c1635-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="c1635-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1635-207">Click **Add** button.</span></span> <span data-ttu-id="c1635-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1635-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="c1635-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c1635-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c1635-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1635-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1635-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1635-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c1635-213">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1635-213">Test single sign-on</span></span>

<span data-ttu-id="c1635-214">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c1635-214">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c1635-215">Po kliknutí na tlačítko hello Predictix řazení dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace pro objednání Predictix.</span><span class="sxs-lookup"><span data-stu-id="c1635-215">When you click hello Predictix Ordering tile in hello Access Panel, you should get automatically signed-on tooyour Predictix Ordering application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c1635-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c1635-216">Additional resources</span></span>

* [<span data-ttu-id="c1635-217">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c1635-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1635-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c1635-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_203.png

