---
title: 'Kurz: Azure Active Directory integrace s Evidence.com | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Evidence.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: eed98c15dca8a6a77f0b5d407bb40ee0037fccad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a><span data-ttu-id="f805d-103">Kurz: Azure Active Directory integrace s Evidence.com</span><span class="sxs-lookup"><span data-stu-id="f805d-103">Tutorial: Azure Active Directory integration with Evidence.com</span></span>

<span data-ttu-id="f805d-104">V tomto kurzu zjistíte, jak toointegrate Evidence.com s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f805d-104">In this tutorial, you learn how toointegrate Evidence.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f805d-105">Integrace Evidence.com s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f805d-105">Integrating Evidence.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f805d-106">Můžete ovládat ve službě Azure AD, který má přístup tooEvidence.com.</span><span class="sxs-lookup"><span data-stu-id="f805d-106">You can control in Azure AD who has access tooEvidence.com.</span></span>
- <span data-ttu-id="f805d-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEvidence.com (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f805d-107">You can enable your users tooautomatically get signed-on tooEvidence.com (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f805d-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f805d-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="f805d-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f805d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f805d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f805d-110">Prerequisites</span></span>

<span data-ttu-id="f805d-111">Integrace služby Azure AD s Evidence.com tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f805d-111">tooconfigure Azure AD integration with Evidence.com, you need hello following items:</span></span>

- <span data-ttu-id="f805d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f805d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f805d-113">Evidence.com jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f805d-113">A Evidence.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f805d-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f805d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f805d-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f805d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f805d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f805d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f805d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f805d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f805d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f805d-118">Scenario description</span></span>
<span data-ttu-id="f805d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f805d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f805d-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f805d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f805d-121">Přidání Evidence.com z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f805d-121">Adding Evidence.com from hello gallery</span></span>
2. <span data-ttu-id="f805d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f805d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evidencecom-from-hello-gallery"></a><span data-ttu-id="f805d-123">Přidání Evidence.com z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f805d-123">Adding Evidence.com from hello gallery</span></span>
<span data-ttu-id="f805d-124">tooconfigure hello integrace Evidence.com do Azure AD, je nutné tooadd Evidence.com hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f805d-124">tooconfigure hello integration of Evidence.com into Azure AD, you need tooadd Evidence.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f805d-125">**tooadd Evidence.com z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f805d-125">**tooadd Evidence.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f805d-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f805d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="f805d-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f805d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f805d-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f805d-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="f805d-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f805d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="f805d-133">Hello vyhledávacího pole zadejte **Evidence.com**, vyberte **Evidence.com** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f805d-133">In hello search box, type **Evidence.com**, select **Evidence.com** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Evidence.com v seznamu výsledků hello](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f805d-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f805d-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f805d-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Evidence.com podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f805d-136">In this section, you configure and test Azure AD single sign-on with Evidence.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f805d-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Evidence.com je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f805d-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Evidence.com is tooa user in Azure AD.</span></span> <span data-ttu-id="f805d-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Evidence.com musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f805d-138">In other words, a link relationship between an Azure AD user and hello related user in Evidence.com needs toobe established.</span></span>

<span data-ttu-id="f805d-139">V Evidence.com, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="f805d-139">In Evidence.com, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f805d-140">tooconfigure a testu Azure AD jednotné přihlašování s Evidence.com, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f805d-140">tooconfigure and test Azure AD single sign-on with Evidence.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f805d-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f805d-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f805d-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f805d-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f805d-143">**[Vytvoření zkušebního uživatele Evidence.com](#create-a-evidencecom-test-user)**  -toohave protějšek Britta Simon v Evidence.com, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f805d-143">**[Create a Evidence.com test user](#create-a-evidencecom-test-user)** - toohave a counterpart of Britta Simon in Evidence.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f805d-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f805d-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f805d-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f805d-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f805d-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f805d-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f805d-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="f805d-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Evidence.com application.</span></span>

<span data-ttu-id="f805d-148">**tooconfigure Azure AD jednotné přihlašování s Evidence.com, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f805d-148">**tooconfigure Azure AD single sign-on with Evidence.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="f805d-149">V portálu Azure, na hello hello **Evidence.com** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f805d-149">In hello Azure portal, on hello **Evidence.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="f805d-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f805d-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. <span data-ttu-id="f805d-153">Na hello **Evidence.com domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f805d-153">On hello **Evidence.com Domain and URLs** section, perform hello following steps:</span></span>

    ![Evidence.com domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_url.png)

    <span data-ttu-id="f805d-155">a.</span><span class="sxs-lookup"><span data-stu-id="f805d-155">a.</span></span> <span data-ttu-id="f805d-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="f805d-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<yourtenant>.evidence.com`</span></span>

    <span data-ttu-id="f805d-157">b.</span><span class="sxs-lookup"><span data-stu-id="f805d-157">b.</span></span> <span data-ttu-id="f805d-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="f805d-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<yourtenant>.evidence.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f805d-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f805d-159">These values are not real.</span></span> <span data-ttu-id="f805d-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f805d-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f805d-161">Obraťte se na [tým podpory Evidence.com klienta](https://communities.taser.com/support/SupportContactUs?typ=LE) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f805d-161">Contact [Evidence.com Client support team](https://communities.taser.com/support/SupportContactUs?typ=LE) tooget these values.</span></span> 

4. <span data-ttu-id="f805d-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f805d-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. <span data-ttu-id="f805d-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f805d-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-evidence-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f805d-166">Na hello **Evidence.com konfigurace** klikněte na tlačítko **konfigurace Evidence.com** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f805d-166">On hello **Evidence.com Configuration** section, click **Configure Evidence.com** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f805d-167">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f805d-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace evidence.com](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_configure.png) 

7. <span data-ttu-id="f805d-169">V okně prohlížeče samostatné webové přihlášení tooyour Evidence.com klienta jako správce a přejděte příliš**správce** karta</span><span class="sxs-lookup"><span data-stu-id="f805d-169">In a separate web browser window, login tooyour Evidence.com tenant as an administrator and navigate too**Admin** Tab</span></span>

8. <span data-ttu-id="f805d-170">Klikněte na **jednotného přihlašování k agentura**</span><span class="sxs-lookup"><span data-stu-id="f805d-170">Click on **Agency Single Sign On**</span></span>

9. <span data-ttu-id="f805d-171">Vyberte **SAML na základě jednotné přihlašování**</span><span class="sxs-lookup"><span data-stu-id="f805d-171">Select **SAML Based Single Sign On**</span></span>

10. <span data-ttu-id="f805d-172">Kopírování hello **SAML Entity ID**, **SAML jeden přihlašování adresa URL služby** a **Sign-Out URL** hodnoty zobrazené v toohello odpovídající pole v Evidence.com a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f805d-172">Copy hello **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** values shown in hello Azure portal and toohello corresponding fields in Evidence.com.</span></span>

11. <span data-ttu-id="f805d-173">Otevřete v poznámkovém bloku hello kopírování obsahu ho do schránky, stažený soubor Certificate(Base64) a pak ji vložit toohello **certifikát zabezpečení** pole.</span><span class="sxs-lookup"><span data-stu-id="f805d-173">Open your downloaded Certificate(Base64) file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Security Certificate** box.</span></span> 

12. <span data-ttu-id="f805d-174">Uložte konfiguraci hello v Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="f805d-174">Save hello configuration in Evidence.com.</span></span>

> [!TIP]
> <span data-ttu-id="f805d-175">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f805d-175">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f805d-176">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f805d-176">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f805d-177">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f805d-177">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f805d-178">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f805d-178">Create an Azure AD test user</span></span>

<span data-ttu-id="f805d-179">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f805d-179">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="f805d-181">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f805d-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f805d-182">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f805d-182">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-evidence-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f805d-184">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f805d-184">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-evidence-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f805d-186">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f805d-186">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-evidence-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f805d-188">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f805d-188">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-evidence-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f805d-190">a.</span><span class="sxs-lookup"><span data-stu-id="f805d-190">a.</span></span> <span data-ttu-id="f805d-191">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f805d-191">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f805d-192">b.</span><span class="sxs-lookup"><span data-stu-id="f805d-192">b.</span></span> <span data-ttu-id="f805d-193">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f805d-193">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="f805d-194">c.</span><span class="sxs-lookup"><span data-stu-id="f805d-194">c.</span></span> <span data-ttu-id="f805d-195">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="f805d-195">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="f805d-196">d.</span><span class="sxs-lookup"><span data-stu-id="f805d-196">d.</span></span> <span data-ttu-id="f805d-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f805d-197">Click **Create**.</span></span>
 
### <a name="create-a-evidencecom-test-user"></a><span data-ttu-id="f805d-198">Vytvoření zkušebního uživatele Evidence.com</span><span class="sxs-lookup"><span data-stu-id="f805d-198">Create a Evidence.com test user</span></span>

<span data-ttu-id="f805d-199">Pro Azure AD Uživatelé toobe možné toosign v musí být zřízená pro přístup k uvnitř hello Evidence.com aplikace.</span><span class="sxs-lookup"><span data-stu-id="f805d-199">For Azure AD users toobe able toosign in, they must be provisioned for access inside hello Evidence.com application.</span></span> <span data-ttu-id="f805d-200">Tato část popisuje, jak toocreate Azure AD uživatelských účtů uvnitř Evidence.com</span><span class="sxs-lookup"><span data-stu-id="f805d-200">This section describes how toocreate Azure AD user accounts inside Evidence.com</span></span>

<span data-ttu-id="f805d-201">**uživatelský účet v Evidence.com tooprovision:**</span><span class="sxs-lookup"><span data-stu-id="f805d-201">**tooprovision a user account in Evidence.com:**</span></span>

1. <span data-ttu-id="f805d-202">V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="f805d-202">In a web browser window, log into your Evidence.com company site as an administrator.</span></span>

2. <span data-ttu-id="f805d-203">Přejděte příliš**správce** kartě.</span><span class="sxs-lookup"><span data-stu-id="f805d-203">Navigate too**Admin** tab.</span></span>

3. <span data-ttu-id="f805d-204">Klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="f805d-204">Click on **Add User**.</span></span>

4. <span data-ttu-id="f805d-205">Klikněte na tlačítko hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f805d-205">Click hello **Add** button.</span></span>

5. <span data-ttu-id="f805d-206">Hello **e-mailovou adresu** z hello přidat, uživatel se musí shodovat uživatelské jméno hello hello uživatelů ve službě Azure AD, který chcete toogive přístup.</span><span class="sxs-lookup"><span data-stu-id="f805d-206">hello **Email Address** of hello added user must match hello username of hello users in Azure AD who you wish toogive access.</span></span> <span data-ttu-id="f805d-207">Pokud hello uživatelské jméno a e-mailové adresy nejsou hello stejnou hodnotu ve vaší organizaci, můžete použít hello **Evidence.com > atributy > jednotné přihlašování** části hello Azure portálu toochange hello nameidenitifer odeslána tooEvidence.com toobe hello e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="f805d-207">If hello username and email address are not hello same value in your organization, you can use hello **Evidence.com > Attributes > Single Sign-On** section of hello Azure portal toochange hello nameidenitifer sent tooEvidence.com toobe hello email address.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f805d-208">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f805d-208">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f805d-209">V této části povolíte tak, že udělíte přístup tooEvidence.com toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f805d-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEvidence.com.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="f805d-211">**tooassign Britta Simon tooEvidence.com, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f805d-211">**tooassign Britta Simon tooEvidence.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="f805d-212">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f805d-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f805d-214">V seznamu aplikace hello vyberte **Evidence.com**.</span><span class="sxs-lookup"><span data-stu-id="f805d-214">In hello applications list, select **Evidence.com**.</span></span>

    ![v seznamu aplikace hello Hello Evidence.com odkaz](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_app.png)  

3. <span data-ttu-id="f805d-216">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f805d-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="f805d-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f805d-218">Click **Add** button.</span></span> <span data-ttu-id="f805d-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f805d-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="f805d-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f805d-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f805d-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f805d-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f805d-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f805d-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f805d-224">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f805d-224">Test single sign-on</span></span>

<span data-ttu-id="f805d-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f805d-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f805d-226">Když kliknete na dlaždici Evidence.com hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Evidence.com aplikace.</span><span class="sxs-lookup"><span data-stu-id="f805d-226">When you click hello Evidence.com tile in hello Access Panel, you should get automatically signed-on tooyour Evidence.com application.</span></span>
<span data-ttu-id="f805d-227">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f805d-227">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f805d-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f805d-228">Additional resources</span></span>

* [<span data-ttu-id="f805d-229">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f805d-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f805d-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f805d-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_203.png

