---
title: 'Kurz: Azure Active Directory integrace s Citrix ShareFile | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Citrix ShareFile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: d7eaf140e56c40f9f621062849dd8558588ffd1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a><span data-ttu-id="884ed-103">Kurz: Azure Active Directory integrace s Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="884ed-103">Tutorial: Azure Active Directory integration with Citrix ShareFile</span></span>

<span data-ttu-id="884ed-104">V tomto kurzu zjistíte, jak toointegrate Citrix ShareFile službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="884ed-104">In this tutorial, you learn how toointegrate Citrix ShareFile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="884ed-105">Integrace Citrix ShareFile s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="884ed-105">Integrating Citrix ShareFile with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="884ed-106">Můžete ovládat ve službě Azure AD, který má přístup tooCitrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="884ed-106">You can control in Azure AD who has access tooCitrix ShareFile.</span></span>
- <span data-ttu-id="884ed-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCitrix ShareFile (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="884ed-107">You can enable your users tooautomatically get signed-on tooCitrix ShareFile (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="884ed-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="884ed-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="884ed-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="884ed-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="884ed-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="884ed-110">Prerequisites</span></span>

<span data-ttu-id="884ed-111">tooconfigure integrace Azure AD s Citrix ShareFile, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="884ed-111">tooconfigure Azure AD integration with Citrix ShareFile, you need hello following items:</span></span>

- <span data-ttu-id="884ed-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="884ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="884ed-113">Citrix ShareFile jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="884ed-113">A Citrix ShareFile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="884ed-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="884ed-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="884ed-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="884ed-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="884ed-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="884ed-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="884ed-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="884ed-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="884ed-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="884ed-118">Scenario description</span></span>
<span data-ttu-id="884ed-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="884ed-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="884ed-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="884ed-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="884ed-121">Přidat Citrix ShareFile z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="884ed-121">Add Citrix ShareFile from hello gallery</span></span>
2. <span data-ttu-id="884ed-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="884ed-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-citrix-sharefile-from-hello-gallery"></a><span data-ttu-id="884ed-123">Přidat Citrix ShareFile z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="884ed-123">Add Citrix ShareFile from hello gallery</span></span>
<span data-ttu-id="884ed-124">tooconfigure hello integrace Citrix ShareFile do Azure AD, je nutné tooadd Citrix ShareFile hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="884ed-124">tooconfigure hello integration of Citrix ShareFile into Azure AD, you need tooadd Citrix ShareFile from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="884ed-125">**tooadd Citrix ShareFile z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="884ed-125">**tooadd Citrix ShareFile from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="884ed-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="884ed-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="884ed-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="884ed-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="884ed-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="884ed-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="884ed-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="884ed-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="884ed-133">Hello vyhledávacího pole zadejte **Citrix ShareFile**, vyberte **Citrix ShareFile** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="884ed-133">In hello search box, type **Citrix ShareFile**, select **Citrix ShareFile** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Citrix ShareFile v hello seznamu výsledků](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="884ed-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="884ed-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="884ed-136">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Citrix ShareFile podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="884ed-136">In this section, you configure and test Azure AD single sign-on with Citrix ShareFile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="884ed-137">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Citrix ShareFile je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="884ed-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Citrix ShareFile is tooa user in Azure AD.</span></span> <span data-ttu-id="884ed-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Citrix ShareFile musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="884ed-138">In other words, a link relationship between an Azure AD user and hello related user in Citrix ShareFile needs toobe established.</span></span>

<span data-ttu-id="884ed-139">V systému Citrix ShareFile přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="884ed-139">In Citrix ShareFile, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="884ed-140">tooconfigure a testu Azure AD jednotné přihlašování s Citrix ShareFile, budete potřebovat následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="884ed-140">tooconfigure and test Azure AD single sign-on with Citrix ShareFile, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="884ed-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="884ed-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="884ed-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="884ed-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="884ed-143">**[Vytvoření zkušebního uživatele Citrix ShareFile](#create-a-citrix-sharefile-test-user)**  -toohave protějšek Britta Simon v Citrix ShareFile, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="884ed-143">**[Create a Citrix ShareFile test user](#create-a-citrix-sharefile-test-user)** - toohave a counterpart of Britta Simon in Citrix ShareFile that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="884ed-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="884ed-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="884ed-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="884ed-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="884ed-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="884ed-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="884ed-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="884ed-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Citrix ShareFile application.</span></span>

<span data-ttu-id="884ed-148">**tooconfigure Azure AD jednotné přihlašování s Citrix ShareFile, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="884ed-148">**tooconfigure Azure AD single sign-on with Citrix ShareFile, perform hello following steps:**</span></span>

1. <span data-ttu-id="884ed-149">V portálu Azure, na hello hello **Citrix ShareFile** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="884ed-149">In hello Azure portal, on hello **Citrix ShareFile** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="884ed-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="884ed-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. <span data-ttu-id="884ed-153">Na hello **Citrix ShareFile domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="884ed-153">On hello **Citrix ShareFile Domain and URLs** section, perform hello following steps:</span></span>

    ![Citrix ShareFile domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    <span data-ttu-id="884ed-155">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.sharefile.com/saml/login`</span><span class="sxs-lookup"><span data-stu-id="884ed-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.sharefile.com/saml/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="884ed-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="884ed-156">This value is not real.</span></span> <span data-ttu-id="884ed-157">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="884ed-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="884ed-158">Obraťte se na [tým podpory Citrix ShareFile klienta](https://www.citrix.co.in/products/sharefile/support.html) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="884ed-158">Contact [Citrix ShareFile Client support team](https://www.citrix.co.in/products/sharefile/support.html) tooget this value.</span></span> 

4. <span data-ttu-id="884ed-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="884ed-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. <span data-ttu-id="884ed-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="884ed-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="884ed-163">Na hello **konfigurace systému Citrix ShareFile** klikněte na tlačítko **konfigurace Citrix ShareFile** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="884ed-163">On hello **Citrix ShareFile Configuration** section, click **Configure Citrix ShareFile** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="884ed-164">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="884ed-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace systému Citrix ShareFile](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. <span data-ttu-id="884ed-166">V okně prohlížeče jiný web, přihlaste se k vaší **Citrix ShareFile** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="884ed-166">In a different web browser window, log into your **Citrix ShareFile** company site as an administrator.</span></span>

8. <span data-ttu-id="884ed-167">V panelu nástrojů hello hello nahoře, klikněte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="884ed-167">In hello toolbar on hello top, click **Admin**.</span></span>

9. <span data-ttu-id="884ed-168">V levém navigačním podokně hello vyberte **nakonfigurovat jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="884ed-168">In hello left navigation pane, select **Configure Single Sign-On**.</span></span>
   
    <span data-ttu-id="884ed-169">![Účet správy](./media/active-directory-saas-sharefile-tutorial/ic773627.png "účet správy")</span><span class="sxs-lookup"><span data-stu-id="884ed-169">![Account Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "Account Administration")</span></span>

10. <span data-ttu-id="884ed-170">Na hello **jednotného přihlašování nebo konfigurace SAML 2.0** dialogové okno stránky v rámci **základní nastavení**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="884ed-170">On hello **Single Sign-On/ SAML 2.0 Configuration** dialog page under **Basic Settings**, perform hello following steps:</span></span>
   
    <span data-ttu-id="884ed-171">![Jednotné přihlašování](./media/active-directory-saas-sharefile-tutorial/ic773628.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="884ed-171">![Single sign-on](./media/active-directory-saas-sharefile-tutorial/ic773628.png "Single sign-on")</span></span>
   
    <span data-ttu-id="884ed-172">a.</span><span class="sxs-lookup"><span data-stu-id="884ed-172">a.</span></span> <span data-ttu-id="884ed-173">Klikněte na tlačítko **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="884ed-173">Click **Enable SAML**.</span></span>
    
    <span data-ttu-id="884ed-174">b.</span><span class="sxs-lookup"><span data-stu-id="884ed-174">b.</span></span> <span data-ttu-id="884ed-175">V **vaše Vystavitel deklarací identity nebo Entity ID** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="884ed-175">In **Your IDP Issuer/ Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="884ed-176">c.</span><span class="sxs-lookup"><span data-stu-id="884ed-176">c.</span></span> <span data-ttu-id="884ed-177">Klikněte na tlačítko **změnu** další toohello **certifikát X.509** pole a pak nahrajte certifikát hello jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="884ed-177">Click **Change** next toohello **X.509 Certificate** field and then upload hello certificate you downloaded from hello Azure portal.</span></span>
    
    <span data-ttu-id="884ed-178">d.</span><span class="sxs-lookup"><span data-stu-id="884ed-178">d.</span></span> <span data-ttu-id="884ed-179">V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="884ed-179">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="884ed-180">e.</span><span class="sxs-lookup"><span data-stu-id="884ed-180">e.</span></span> <span data-ttu-id="884ed-181">V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="884ed-181">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

11. <span data-ttu-id="884ed-182">Klikněte na tlačítko **Uložit** na hello Citrix ShareFile portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="884ed-182">Click **Save** on hello Citrix ShareFile management portal.</span></span>

> [!TIP]
> <span data-ttu-id="884ed-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="884ed-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="884ed-184">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="884ed-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="884ed-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="884ed-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="884ed-186">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="884ed-186">Create an Azure AD test user</span></span>

<span data-ttu-id="884ed-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="884ed-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="884ed-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="884ed-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="884ed-190">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="884ed-190">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="884ed-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="884ed-192">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="884ed-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="884ed-194">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="884ed-196">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="884ed-196">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    <span data-ttu-id="884ed-198">a.</span><span class="sxs-lookup"><span data-stu-id="884ed-198">a.</span></span> <span data-ttu-id="884ed-199">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="884ed-199">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="884ed-200">b.</span><span class="sxs-lookup"><span data-stu-id="884ed-200">b.</span></span> <span data-ttu-id="884ed-201">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="884ed-201">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="884ed-202">c.</span><span class="sxs-lookup"><span data-stu-id="884ed-202">c.</span></span> <span data-ttu-id="884ed-203">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="884ed-203">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="884ed-204">d.</span><span class="sxs-lookup"><span data-stu-id="884ed-204">d.</span></span> <span data-ttu-id="884ed-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="884ed-205">Click **Create**.</span></span>
 
### <a name="create-a-citrix-sharefile-test-user"></a><span data-ttu-id="884ed-206">Vytvoření zkušebního uživatele Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="884ed-206">Create a Citrix ShareFile test user</span></span>

<span data-ttu-id="884ed-207">V pořadí tooenable Azure AD Uživatelé toolog do Citrix ShareFile musí být zřízená do Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="884ed-207">In order tooenable Azure AD users toolog into Citrix ShareFile, they must be provisioned into Citrix ShareFile.</span></span> <span data-ttu-id="884ed-208">V případě hello Citrix ShareFile zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="884ed-208">In hello case of Citrix ShareFile, provisioning is a manual task.</span></span>

<span data-ttu-id="884ed-209">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="884ed-209">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="884ed-210">Přihlaste se tooyour **Citrix ShareFile** klienta.</span><span class="sxs-lookup"><span data-stu-id="884ed-210">Log in tooyour **Citrix ShareFile** tenant.</span></span>

2. <span data-ttu-id="884ed-211">Klikněte na tlačítko **Správa uživatelů \> spravovat uživatelé domovské \> + vytvořit zaměstnanec**.</span><span class="sxs-lookup"><span data-stu-id="884ed-211">Click **Manage Users \> Manage Users Home \> + Create Employee**.</span></span>
   
   <span data-ttu-id="884ed-212">![Vytvoření zaměstnance](./media/active-directory-saas-sharefile-tutorial/IC781050.png "vytvoření zaměstnance")</span><span class="sxs-lookup"><span data-stu-id="884ed-212">![Create Employee](./media/active-directory-saas-sharefile-tutorial/IC781050.png "Create Employee")</span></span>

3. <span data-ttu-id="884ed-213">Na hello **základní informace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="884ed-213">On hello **Basic Information** section, perform below steps:</span></span>
   
   <span data-ttu-id="884ed-214">![Základní informace](./media/active-directory-saas-sharefile-tutorial/IC799951.png "základní informace")</span><span class="sxs-lookup"><span data-stu-id="884ed-214">![Basic Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "Basic Information")</span></span>
   
   <span data-ttu-id="884ed-215">a.</span><span class="sxs-lookup"><span data-stu-id="884ed-215">a.</span></span> <span data-ttu-id="884ed-216">V hello **e-mailovou adresu** textovému poli, typ hello e-mailovou adresu Britta Simon jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="884ed-216">In hello **Email Address** textbox, type hello email address of Britta Simon as **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="884ed-217">b.</span><span class="sxs-lookup"><span data-stu-id="884ed-217">b.</span></span> <span data-ttu-id="884ed-218">V hello **křestní jméno** textovému poli, typ **křestní jméno** uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="884ed-218">In hello **First Name** textbox, type **first name** of user as **Britta**.</span></span>
   
   <span data-ttu-id="884ed-219">c.</span><span class="sxs-lookup"><span data-stu-id="884ed-219">c.</span></span> <span data-ttu-id="884ed-220">V hello **příjmení** textovému poli, typ **příjmení** uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="884ed-220">In hello **Last Name** textbox, type **last name** of user as **Simon**.</span></span>

4. <span data-ttu-id="884ed-221">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="884ed-221">Click **Add User**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="884ed-222">Držitel účtu Azure AD Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní. Můžete použít všechny ostatní Citrix ShareFile uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Citrix ShareFile tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="884ed-222">hello Azure AD account holder will receive an email and follow a link tooconfirm their account before it becomes active.You can use any other Citrix ShareFile user account creation tools or APIs provided by Citrix ShareFile tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="884ed-223">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="884ed-223">Assign hello Azure AD test user</span></span>

<span data-ttu-id="884ed-224">V této části povolíte tak, že udělíte přístup tooCitrix ShareFile toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="884ed-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCitrix ShareFile.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="884ed-226">**tooassign Britta Simon tooCitrix ShareFile, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="884ed-226">**tooassign Britta Simon tooCitrix ShareFile, perform hello following steps:**</span></span>

1. <span data-ttu-id="884ed-227">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="884ed-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="884ed-229">V seznamu aplikace hello vyberte **Citrix ShareFile**.</span><span class="sxs-lookup"><span data-stu-id="884ed-229">In hello applications list, select **Citrix ShareFile**.</span></span>

    ![Hello Citrix ShareFile odkaz v seznamu aplikace hello](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. <span data-ttu-id="884ed-231">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="884ed-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="884ed-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="884ed-233">Click **Add** button.</span></span> <span data-ttu-id="884ed-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="884ed-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="884ed-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="884ed-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="884ed-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="884ed-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="884ed-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="884ed-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="884ed-239">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="884ed-239">Test single sign-on</span></span>

<span data-ttu-id="884ed-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="884ed-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="884ed-241">Po kliknutí na tlačítko hello Citrix ShareFile dlaždici v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného Citrix ShareFile aplikace.</span><span class="sxs-lookup"><span data-stu-id="884ed-241">When you click hello Citrix ShareFile tile in hello Access Panel, you should get automatically signed-on tooyour Citrix ShareFile application.</span></span>
<span data-ttu-id="884ed-242">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="884ed-242">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="884ed-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="884ed-243">Additional resources</span></span>

* [<span data-ttu-id="884ed-244">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="884ed-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="884ed-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="884ed-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

