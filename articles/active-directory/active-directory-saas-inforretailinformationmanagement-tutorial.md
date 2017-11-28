---
title: "Kurz: Azure Active Directory integrace s prodejní informací – správu informace | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a prodejní informací – informace správy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 9cd8ab65d41d01832e0611f7f8254aa257120508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a><span data-ttu-id="d2fd0-103">Kurz: Azure Active Directory integrace s prodejní informací – informace správy</span><span class="sxs-lookup"><span data-stu-id="d2fd0-103">Tutorial: Azure Active Directory integration with Infor Retail – Information Management</span></span>

<span data-ttu-id="d2fd0-104">V tomto kurzu zjistíte, jak toointegrate prodejní informací – informace správy pomocí služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d2fd0-104">In this tutorial, you learn how toointegrate Infor Retail – Information Management with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d2fd0-105">Integrace informací prodejní – informace správy s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-105">Integrating Infor Retail – Information Management with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d2fd0-106">Můžete ovládat ve službě Azure AD, který má přístup tooInfor prodejní – informace správy.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-106">You can control in Azure AD who has access tooInfor Retail – Information Management.</span></span>
- <span data-ttu-id="d2fd0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooInfor prodejní – správy informací (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-107">You can enable your users tooautomatically get signed-on tooInfor Retail – Information Management (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d2fd0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="d2fd0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d2fd0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2fd0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2fd0-110">Prerequisites</span></span>

<span data-ttu-id="d2fd0-111">tooconfigure integrace Azure AD s prodejní informací – informace správy, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-111">tooconfigure Azure AD integration with Infor Retail – Information Management, you need hello following items:</span></span>

- <span data-ttu-id="d2fd0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2fd0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d2fd0-113">Prodejní informací – informace správy jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d2fd0-113">An Infor Retail – Information Management single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d2fd0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d2fd0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d2fd0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d2fd0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d2fd0-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d2fd0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d2fd0-118">Scenario description</span></span>
<span data-ttu-id="d2fd0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d2fd0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d2fd0-121">Přidání informací prodejní – správu informace z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d2fd0-121">Adding Infor Retail – Information Management from hello gallery</span></span>
2. <span data-ttu-id="d2fd0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2fd0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-infor-retail--information-management-from-hello-gallery"></a><span data-ttu-id="d2fd0-123">Přidání informací prodejní – správu informace z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d2fd0-123">Adding Infor Retail – Information Management from hello gallery</span></span>
<span data-ttu-id="d2fd0-124">integrace hello tooconfigure informací prodejní – informace správy do služby Azure AD, je nutné tooadd informací prodejní – informace o správu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-124">tooconfigure hello integration of Infor Retail – Information Management into Azure AD, you need tooadd Infor Retail – Information Management from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d2fd0-125">**tooadd informací prodejní – správu informace z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d2fd0-125">**tooadd Infor Retail – Information Management from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2fd0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="d2fd0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d2fd0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="d2fd0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="d2fd0-133">Hello vyhledávacího pole zadejte **informací prodejní – informace o správu**, vyberte **informací prodejní – informace o správu** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-133">In hello search box, type **Infor Retail – Information Management**, select **Infor Retail – Information Management** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Prodejní informací – informace správy v seznamu výsledků hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d2fd0-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2fd0-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d2fd0-136">V této části konfiguraci a testování Azure AD jednotné přihlašování s prodejní informací – informace správy na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d2fd0-136">In this section, you configure and test Azure AD single sign-on with Infor Retail – Information Management based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d2fd0-137">Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v prodejní informací – informace správy je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Infor Retail – Information Management is tooa user in Azure AD.</span></span> <span data-ttu-id="d2fd0-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v prodejní informací – informace správy musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-138">In other words, a link relationship between an Azure AD user and hello related user in Infor Retail – Information Management needs toobe established.</span></span>

<span data-ttu-id="d2fd0-139">V prodejní informací – informace o správě přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-139">In Infor Retail – Information Management, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d2fd0-140">tooconfigure a testování Azure AD jednotné přihlašování s prodejní informací – informace správy, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-140">tooconfigure and test Azure AD single sign-on with Infor Retail – Information Management, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d2fd0-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d2fd0-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d2fd0-143">**[Vytvoření informací prodejní – informace o správu testovacího uživatele](#create-an-infor-retail--information-management-test-user)**  - toohave protějšek Britta Simon v prodejní informací – Správa informace, který je reprezentace toohello propojené služby Azure AD uživatele.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-143">**[Create an Infor Retail – Information Management test user](#create-an-infor-retail--information-management-test-user)** - toohave a counterpart of Britta Simon in Infor Retail – Information Management that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d2fd0-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d2fd0-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d2fd0-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2fd0-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d2fd0-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v prodejní informací – informace správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Infor Retail – Information Management application.</span></span>

<span data-ttu-id="d2fd0-148">**tooconfigure Azure AD jednotné přihlašování s prodejní informací – informace o správě proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d2fd0-148">**tooconfigure Azure AD single sign-on with Infor Retail – Information Management, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2fd0-149">V portálu Azure, na hello hello **informací prodejní – informace o správu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-149">In hello Azure portal, on hello **Infor Retail – Information Management** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="d2fd0-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

3. <span data-ttu-id="d2fd0-153">Na hello **informací prodejní – informace o doméně pro správu a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure v rozšíření IDP iniciované režimu hello:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-153">On hello **Infor Retail – Information Management Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in IDP initiated mode:</span></span>

    ![Jednotné přihlašování informace IDP informací prodejní – informace o doméně pro správu a adresy URL](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    <span data-ttu-id="d2fd0-155">a.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-155">a.</span></span> <span data-ttu-id="d2fd0-156">V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-156">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    <span data-ttu-id="d2fd0-157">b.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-157">b.</span></span> <span data-ttu-id="d2fd0-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.mingle.infor.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="d2fd0-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.mingle.infor.com/sp/ACS.saml2`</span></span>

4. <span data-ttu-id="d2fd0-159">Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující krok, pokud chcete aplikace hello tooconfigure hello **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-159">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Prodejní informací – informace o doméně pro správu a adresy URL jeden přihlašování informace SP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    <span data-ttu-id="d2fd0-161">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.mingle.infor.com/<company code>`</span><span class="sxs-lookup"><span data-stu-id="d2fd0-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.mingle.infor.com/<company code>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="d2fd0-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-162">These values are not real.</span></span> <span data-ttu-id="d2fd0-163">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="d2fd0-164">Obraťte se na [informací prodejní – tým podpory klienta pro správu informace](mailto:innovate@infor.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-164">Contact [Infor Retail – Information Management Client support team](mailto:innovate@infor.com) tooget these values.</span></span> 

5. <span data-ttu-id="d2fd0-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

6. <span data-ttu-id="d2fd0-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-167">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="d2fd0-169">tooconfigure jednotného přihlašování na **informací prodejní – informace o správu** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[informací prodejní – tým podpory informace správy ](mailto:innovate@infor.com).</span><span class="sxs-lookup"><span data-stu-id="d2fd0-169">tooconfigure single sign-on on **Infor Retail – Information Management** side, you need toosend hello downloaded **Metadata XML** too[Infor Retail – Information Management support team](mailto:innovate@infor.com).</span></span> <span data-ttu-id="d2fd0-170">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d2fd0-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d2fd0-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d2fd0-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d2fd0-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d2fd0-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d2fd0-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2fd0-174">Create an Azure AD test user</span></span>

<span data-ttu-id="d2fd0-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="d2fd0-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d2fd0-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2fd0-178">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d2fd0-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d2fd0-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d2fd0-184">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d2fd0-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d2fd0-186">a.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-186">a.</span></span> <span data-ttu-id="d2fd0-187">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d2fd0-188">b.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-188">b.</span></span> <span data-ttu-id="d2fd0-189">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="d2fd0-190">c.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-190">c.</span></span> <span data-ttu-id="d2fd0-191">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="d2fd0-192">d.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-192">d.</span></span> <span data-ttu-id="d2fd0-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-193">Click **Create**.</span></span>
 
### <a name="create-an-infor-retail--information-management-test-user"></a><span data-ttu-id="d2fd0-194">Vytvoření informací prodejní – informace o správu testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d2fd0-194">Create an Infor Retail – Information Management test user</span></span>

<span data-ttu-id="d2fd0-195">V této části vytvoříte uživatele volal Britta Simon v prodejní informací – informace správy.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-195">In this section, you create a user called Britta Simon in Infor Retail – Information Management.</span></span> <span data-ttu-id="d2fd0-196">Spojte se s [informací prodejní – tým podpory správy informace](mailto:innovate@infor.com) tooadd hello uživatele v hello informací prodejní – platforma pro správu informace.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-196">Please work with [Infor Retail – Information Management support team](mailto:innovate@infor.com) tooadd hello users in hello Infor Retail – Information Management platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="d2fd0-197">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d2fd0-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="d2fd0-198">V této části povolíte tak, že udělíte přístup tooInfor prodejní – informace o správu Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInfor Retail – Information Management.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="d2fd0-200">**tooassign Britta Simon tooInfor prodejní – informace o správě proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d2fd0-200">**tooassign Britta Simon tooInfor Retail – Information Management, perform hello following steps:**</span></span>

1. <span data-ttu-id="d2fd0-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d2fd0-203">V seznamu aplikace hello vyberte **informací prodejní – informace o správu**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-203">In hello applications list, select **Infor Retail – Information Management**.</span></span>

    ![Hello informací prodejní – informace správy na odkaz v seznamu aplikace hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

3. <span data-ttu-id="d2fd0-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="d2fd0-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-207">Click **Add** button.</span></span> <span data-ttu-id="d2fd0-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="d2fd0-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d2fd0-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d2fd0-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d2fd0-213">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d2fd0-213">Test single sign-on</span></span>

<span data-ttu-id="d2fd0-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d2fd0-215">Po kliknutí na tlačítko hello informací prodejní – informace správy dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour prodejní informací – informace správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="d2fd0-215">When you click hello Infor Retail – Information Management tile in hello Access Panel, you should get automatically signed-on tooyour Infor Retail – Information Management application.</span></span>
<span data-ttu-id="d2fd0-216">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d2fd0-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d2fd0-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d2fd0-217">Additional resources</span></span>

* [<span data-ttu-id="d2fd0-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d2fd0-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d2fd0-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d2fd0-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_203.png

