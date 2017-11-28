---
title: 'Kurz: Azure Active Directory integrace s TargetProcess | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TargetProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 05c574e2c18d7f73edc6c094093a6e59d46b8e6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a><span data-ttu-id="bdc87-103">Kurz: Azure Active Directory integrace s TargetProcess</span><span class="sxs-lookup"><span data-stu-id="bdc87-103">Tutorial: Azure Active Directory integration with TargetProcess</span></span>

<span data-ttu-id="bdc87-104">V tomto kurzu zjistíte, jak toointegrate TargetProcess s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bdc87-104">In this tutorial, you learn how toointegrate TargetProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bdc87-105">Integrace TargetProcess s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bdc87-105">Integrating TargetProcess with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bdc87-106">Můžete řídit ve službě Azure AD, který má přístup tooTargetProcess</span><span class="sxs-lookup"><span data-stu-id="bdc87-106">You can control in Azure AD who has access tooTargetProcess</span></span>
- <span data-ttu-id="bdc87-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTargetProcess (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="bdc87-107">You can enable your users tooautomatically get signed-on tooTargetProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bdc87-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bdc87-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bdc87-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bdc87-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdc87-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bdc87-110">Prerequisites</span></span>

<span data-ttu-id="bdc87-111">Integrace služby Azure AD s TargetProcess tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="bdc87-111">tooconfigure Azure AD integration with TargetProcess, you need hello following items:</span></span>

- <span data-ttu-id="bdc87-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bdc87-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bdc87-113">TargetProcess jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bdc87-113">A TargetProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bdc87-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bdc87-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bdc87-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bdc87-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bdc87-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bdc87-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bdc87-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bdc87-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bdc87-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bdc87-118">Scenario description</span></span>
<span data-ttu-id="bdc87-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bdc87-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bdc87-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bdc87-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bdc87-121">Přidat TargetProcess z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="bdc87-121">Add TargetProcess from hello gallery</span></span>
2. <span data-ttu-id="bdc87-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bdc87-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-targetprocess-from-hello-gallery"></a><span data-ttu-id="bdc87-123">Přidat TargetProcess z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="bdc87-123">Add TargetProcess from hello gallery</span></span>
<span data-ttu-id="bdc87-124">tooconfigure hello integrace TargetProcess do Azure AD, je nutné tooadd TargetProcess hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="bdc87-124">tooconfigure hello integration of TargetProcess into Azure AD, you need tooadd TargetProcess from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bdc87-125">**tooadd TargetProcess z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bdc87-125">**tooadd TargetProcess from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bdc87-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bdc87-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bdc87-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bdc87-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="bdc87-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bdc87-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="bdc87-133">Hello vyhledávacího pole zadejte **TargetProcess**, vyberte **TargetProcess** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdc87-133">In hello search box, type **TargetProcess**, select **TargetProcess**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Přidat TargetProcess z Galerie](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bdc87-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bdc87-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="bdc87-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TargetProcess podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="bdc87-136">In this section, you configure and test Azure AD single sign-on with TargetProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bdc87-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TargetProcess je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bdc87-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TargetProcess is tooa user in Azure AD.</span></span> <span data-ttu-id="bdc87-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TargetProcess musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="bdc87-138">In other words, a link relationship between an Azure AD user and hello related user in TargetProcess needs toobe established.</span></span>

<span data-ttu-id="bdc87-139">V TargetProcess, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="bdc87-139">In TargetProcess, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bdc87-140">tooconfigure a testu Azure AD jednotné přihlašování s TargetProcess, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="bdc87-140">tooconfigure and test Azure AD single sign-on with TargetProcess, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bdc87-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="bdc87-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bdc87-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bdc87-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bdc87-143">**[Vytvoření zkušebního uživatele TargetProcess](#create-a-targetprocess-test-user)**  -toohave protějšek Britta Simon v TargetProcess, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="bdc87-143">**[Create a TargetProcess test user](#create-a-targetprocess-test-user)** - toohave a counterpart of Britta Simon in TargetProcess that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bdc87-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bdc87-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bdc87-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="bdc87-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="bdc87-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bdc87-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="bdc87-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="bdc87-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TargetProcess application.</span></span>

<span data-ttu-id="bdc87-148">**tooconfigure Azure AD jednotné přihlašování s TargetProcess, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bdc87-148">**tooconfigure Azure AD single sign-on with TargetProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="bdc87-149">V portálu Azure, na hello hello **TargetProcess** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-149">In hello Azure portal, on hello **TargetProcess** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="bdc87-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bdc87-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. <span data-ttu-id="bdc87-153">Na hello **TargetProcess domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bdc87-153">On hello **TargetProcess Domain and URLs** section, perform hello following steps:</span></span>

    ![Část TargetProcess domény a adresy URL](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    <span data-ttu-id="bdc87-155">a.</span><span class="sxs-lookup"><span data-stu-id="bdc87-155">a.</span></span> <span data-ttu-id="bdc87-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="bdc87-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    <span data-ttu-id="bdc87-157">b.</span><span class="sxs-lookup"><span data-stu-id="bdc87-157">b.</span></span> <span data-ttu-id="bdc87-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="bdc87-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bdc87-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="bdc87-159">These values are not real.</span></span> <span data-ttu-id="bdc87-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="bdc87-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="bdc87-161">Obraťte se na [tým podpory TargetProcess klienta](mailto:support@targetprocess.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bdc87-161">Contact [TargetProcess Client support team](mailto:support@targetprocess.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="bdc87-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bdc87-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. <span data-ttu-id="bdc87-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bdc87-164">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bdc87-166">Na hello **TargetProcess konfigurace** klikněte na tlačítko **konfigurace TargetProcess** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="bdc87-166">On hello **TargetProcess Configuration** section, click **Configure TargetProcess** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bdc87-167">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="bdc87-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TargetProcess konfigurační oddíl](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. <span data-ttu-id="bdc87-169">Přihlášení tooyour TargetProcess aplikace jako správce.</span><span class="sxs-lookup"><span data-stu-id="bdc87-169">Sign-on tooyour TargetProcess application as an administrator.</span></span>

8. <span data-ttu-id="bdc87-170">V nabídce hello hello nahoře, klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-170">In hello menu on hello top, click **Setup**.</span></span>
   
    ![Nastavení](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. <span data-ttu-id="bdc87-172">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-172">Click **Settings**.</span></span>
   
    ![Nastavení](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. <span data-ttu-id="bdc87-174">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-174">Click **Single Sign-on**.</span></span>
   
    ![Klikněte na jednotné přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. <span data-ttu-id="bdc87-176">V hello jednotné přihlašování v dialogu nastavení proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bdc87-176">On hello Single Sign-on settings dialog, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    <span data-ttu-id="bdc87-178">a.</span><span class="sxs-lookup"><span data-stu-id="bdc87-178">a.</span></span> <span data-ttu-id="bdc87-179">Klikněte na tlačítko **povolit jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-179">Click **Enable Single Sign-on**.</span></span>
    
    <span data-ttu-id="bdc87-180">b.</span><span class="sxs-lookup"><span data-stu-id="bdc87-180">b.</span></span> <span data-ttu-id="bdc87-181">V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bdc87-181">In **Sign-on URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bdc87-182">c.</span><span class="sxs-lookup"><span data-stu-id="bdc87-182">c.</span></span> <span data-ttu-id="bdc87-183">Otevřete stažený certifikát v poznámkovém bloku hello kopírování obsahu a pak ji vložit do hello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="bdc87-183">Open your downloaded certificate in notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span>
    
    <span data-ttu-id="bdc87-184">d.</span><span class="sxs-lookup"><span data-stu-id="bdc87-184">d.</span></span> <span data-ttu-id="bdc87-185">Klikněte na tlačítko **povolit zřizování JIT**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-185">click **Enable JIT Provisioning**.</span></span>

    <span data-ttu-id="bdc87-186">e.</span><span class="sxs-lookup"><span data-stu-id="bdc87-186">e.</span></span> <span data-ttu-id="bdc87-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="bdc87-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="bdc87-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bdc87-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="bdc87-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bdc87-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bdc87-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bdc87-191">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bdc87-191">Create an Azure AD test user</span></span>
<span data-ttu-id="bdc87-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="bdc87-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="bdc87-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bdc87-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bdc87-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bdc87-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bdc87-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![toodisplay hello seznam uživatelů](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bdc87-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="bdc87-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bdc87-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bdc87-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Oddíl uživatele](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bdc87-203">a.</span><span class="sxs-lookup"><span data-stu-id="bdc87-203">a.</span></span> <span data-ttu-id="bdc87-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bdc87-205">b.</span><span class="sxs-lookup"><span data-stu-id="bdc87-205">b.</span></span> <span data-ttu-id="bdc87-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bdc87-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bdc87-207">c.</span><span class="sxs-lookup"><span data-stu-id="bdc87-207">c.</span></span> <span data-ttu-id="bdc87-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bdc87-209">d.</span><span class="sxs-lookup"><span data-stu-id="bdc87-209">d.</span></span> <span data-ttu-id="bdc87-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-210">Click **Create**.</span></span>
 
### <a name="create-a-targetprocess-test-user"></a><span data-ttu-id="bdc87-211">Vytvoření zkušebního uživatele TargetProcess</span><span class="sxs-lookup"><span data-stu-id="bdc87-211">Create a TargetProcess test user</span></span>

<span data-ttu-id="bdc87-212">Hello cílem této části je toocreate volal Britta Simon v TargetProcess uživatele.</span><span class="sxs-lookup"><span data-stu-id="bdc87-212">hello objective of this section is toocreate a user called Britta Simon in TargetProcess.</span></span>

<span data-ttu-id="bdc87-213">TargetProcess podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="bdc87-213">TargetProcess supports just-in-time provisioning.</span></span> <span data-ttu-id="bdc87-214">Již je v povolíte [konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="bdc87-214">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="bdc87-215">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="bdc87-215">There is no action item for you in this section.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="bdc87-216">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="bdc87-216">Assign hello Azure AD test user</span></span>

<span data-ttu-id="bdc87-217">V této části povolíte tak, že udělíte přístup tooTargetProcess toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bdc87-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTargetProcess.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="bdc87-219">**tooassign Britta Simon tooTargetProcess, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bdc87-219">**tooassign Britta Simon tooTargetProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="bdc87-220">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bdc87-222">V seznamu aplikace hello vyberte **TargetProcess**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-222">In hello applications list, select **TargetProcess**.</span></span>

    ![TargetProcess v seznamu aplikací](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. <span data-ttu-id="bdc87-224">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bdc87-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="bdc87-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bdc87-226">Click **Add** button.</span></span> <span data-ttu-id="bdc87-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bdc87-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="bdc87-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="bdc87-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bdc87-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bdc87-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bdc87-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bdc87-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="bdc87-232">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bdc87-232">Test single sign-on</span></span>

<span data-ttu-id="bdc87-233">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bdc87-233">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bdc87-234">Po kliknutí na tlačítko hello TargetProcess dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour TargetProcess aplikace.</span><span class="sxs-lookup"><span data-stu-id="bdc87-234">When you click hello TargetProcess tile in hello Access Panel, you should get automatically signed-on tooyour TargetProcess application.</span></span> <span data-ttu-id="bdc87-235">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bdc87-235">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bdc87-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bdc87-236">Additional resources</span></span>

* [<span data-ttu-id="bdc87-237">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bdc87-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bdc87-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bdc87-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

