---
title: 'Kurz: Azure Active Directory integrace s FileCloud | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FileCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f39f0ddd-b504-4562-971f-77b88d1e75fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: fe58d01f02d6ce99ee9e2f83e7dc72c4434e13b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a><span data-ttu-id="069e4-103">Kurz: Azure Active Directory integrace s FileCloud</span><span class="sxs-lookup"><span data-stu-id="069e4-103">Tutorial: Azure Active Directory integration with FileCloud</span></span>

<span data-ttu-id="069e4-104">V tomto kurzu zjistíte, jak toointegrate FileCloud s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="069e4-104">In this tutorial, you learn how toointegrate FileCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="069e4-105">Integrace FileCloud s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="069e4-105">Integrating FileCloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="069e4-106">Můžete ovládat ve službě Azure AD, který má přístup tooFileCloud.</span><span class="sxs-lookup"><span data-stu-id="069e4-106">You can control in Azure AD who has access tooFileCloud.</span></span>
- <span data-ttu-id="069e4-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFileCloud (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="069e4-107">You can enable your users tooautomatically get signed-on tooFileCloud (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="069e4-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="069e4-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="069e4-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="069e4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="069e4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="069e4-110">Prerequisites</span></span>

<span data-ttu-id="069e4-111">Integrace služby Azure AD s FileCloud tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="069e4-111">tooconfigure Azure AD integration with FileCloud, you need hello following items:</span></span>

- <span data-ttu-id="069e4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="069e4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="069e4-113">FileCloud jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="069e4-113">A FileCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="069e4-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="069e4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="069e4-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="069e4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="069e4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="069e4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="069e4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="069e4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="069e4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="069e4-118">Scenario description</span></span>
<span data-ttu-id="069e4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="069e4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="069e4-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="069e4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="069e4-121">Přidání FileCloud z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="069e4-121">Adding FileCloud from hello gallery</span></span>
2. <span data-ttu-id="069e4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="069e4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-filecloud-from-hello-gallery"></a><span data-ttu-id="069e4-123">Přidání FileCloud z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="069e4-123">Adding FileCloud from hello gallery</span></span>
<span data-ttu-id="069e4-124">tooconfigure hello integrace FileCloud do Azure AD, je nutné tooadd FileCloud hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="069e4-124">tooconfigure hello integration of FileCloud into Azure AD, you need tooadd FileCloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="069e4-125">**tooadd FileCloud z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="069e4-125">**tooadd FileCloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="069e4-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="069e4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="069e4-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="069e4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="069e4-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="069e4-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="069e4-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="069e4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="069e4-133">Hello vyhledávacího pole zadejte **FileCloud**, vyberte **FileCloud** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="069e4-133">In hello search box, type **FileCloud**, select **FileCloud** from result panel then click **Add** button tooadd hello application.</span></span>

    ![FileCloud v seznamu výsledků hello](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="069e4-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="069e4-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="069e4-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FileCloud podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="069e4-136">In this section, you configure and test Azure AD single sign-on with FileCloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="069e4-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FileCloud je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="069e4-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FileCloud is tooa user in Azure AD.</span></span> <span data-ttu-id="069e4-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FileCloud musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="069e4-138">In other words, a link relationship between an Azure AD user and hello related user in FileCloud needs toobe established.</span></span>

<span data-ttu-id="069e4-139">V FileCloud, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="069e4-139">In FileCloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="069e4-140">tooconfigure a testu Azure AD jednotné přihlašování s FileCloud, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="069e4-140">tooconfigure and test Azure AD single sign-on with FileCloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="069e4-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="069e4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="069e4-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="069e4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="069e4-143">**[Vytvoření zkušebního uživatele FileCloud](#create-a-filecloud-test-user)**  -toohave protějšek Britta Simon v FileCloud, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="069e4-143">**[Create a FileCloud test user](#create-a-filecloud-test-user)** - toohave a counterpart of Britta Simon in FileCloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="069e4-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="069e4-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="069e4-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="069e4-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="069e4-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="069e4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="069e4-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci FileCloud.</span><span class="sxs-lookup"><span data-stu-id="069e4-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your FileCloud application.</span></span>

<span data-ttu-id="069e4-148">**tooconfigure Azure AD jednotné přihlašování s FileCloud, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="069e4-148">**tooconfigure Azure AD single sign-on with FileCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="069e4-149">V portálu Azure, na hello hello **FileCloud** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="069e4-149">In hello Azure portal, on hello **FileCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="069e4-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="069e4-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_samlbase.png)

3. <span data-ttu-id="069e4-153">Na hello **FileCloud domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="069e4-153">On hello **FileCloud Domain and URLs** section, perform hello following steps:</span></span>

    ![FileCloud domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_url.png)

    <span data-ttu-id="069e4-155">a.</span><span class="sxs-lookup"><span data-stu-id="069e4-155">a.</span></span> <span data-ttu-id="069e4-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.filecloudhosted.com`</span><span class="sxs-lookup"><span data-stu-id="069e4-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.filecloudhosted.com`</span></span>

    <span data-ttu-id="069e4-157">b.</span><span class="sxs-lookup"><span data-stu-id="069e4-157">b.</span></span> <span data-ttu-id="069e4-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span><span class="sxs-lookup"><span data-stu-id="069e4-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="069e4-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="069e4-159">These values are not real.</span></span> <span data-ttu-id="069e4-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="069e4-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="069e4-161">Obraťte se na [tým podpory FileCloud klienta](mailto:support@codelathe.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="069e4-161">Contact [FileCloud Client support team](mailto:support@codelathe.com) tooget these values.</span></span>

4. <span data-ttu-id="069e4-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="069e4-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_certificate.png) 

5. <span data-ttu-id="069e4-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="069e4-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-filecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="069e4-166">Na hello **FileCloud konfigurace** klikněte na tlačítko **konfigurace FileCloud** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="069e4-166">On hello **FileCloud Configuration** section, click **Configure FileCloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="069e4-167">Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="069e4-167">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurace FileCloud](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_configure.png) 

7. <span data-ttu-id="069e4-169">V okně prohlížeče jiný web, klienta FileCloud tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="069e4-169">In a different web browser window, sign-on tooyour FileCloud tenant as an administrator.</span></span>

8. <span data-ttu-id="069e4-170">V levém navigačním podokně hello, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="069e4-170">On hello left navigation pane, click **Settings**.</span></span> 
   
    ![Nastavení oddílu aplikace na straně](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

9. <span data-ttu-id="069e4-172">Klikněte na tlačítko **jednotného přihlašování k** karty v části nastavení.</span><span class="sxs-lookup"><span data-stu-id="069e4-172">Click **SSO** tab on Settings section.</span></span> 
   
    ![Jeden straně přihlašování karta na aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

10. <span data-ttu-id="069e4-174">Vyberte **SAML** jako **výchozí typ jednotného přihlašování k** na **jednotné přihlašování na (SSO) nastavení** panelu.</span><span class="sxs-lookup"><span data-stu-id="069e4-174">Select **SAML** as **Default SSO Type** on **Single Sign On (SSO) Settings** panel.</span></span>
   
    ![Jeden straně přihlašování nastavení panely na aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

11. <span data-ttu-id="069e4-176">Vložení **SAML Entity ID**, který jste zkopírovali z portálu Azure do hello **adresa URL koncového bodu IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="069e4-176">Paste **SAML Entity ID**, which you have copied from Azure portal into hello **IdP End Point URL** textbox.</span></span>

    ![Adresa URL bodu IDP End textové pole](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

12. <span data-ttu-id="069e4-178">Otevřete váš soubor stažený metadata v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **IdP metadata** textové pole na **SAML nastavení** panelu.</span><span class="sxs-lookup"><span data-stu-id="069e4-178">Open your downloaded metadata file in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Meta Data** textbox on **SAML Settings** panel.</span></span>

    ![Část dat Meta IDP na straně aplikace](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

13. <span data-ttu-id="069e4-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="069e4-180">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="069e4-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="069e4-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="069e4-182">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="069e4-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="069e4-183">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="069e4-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="069e4-184">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="069e4-184">Create an Azure AD test user</span></span>

<span data-ttu-id="069e4-185">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="069e4-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="069e4-187">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="069e4-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="069e4-188">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="069e4-188">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-filecloud-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="069e4-190">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="069e4-190">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-filecloud-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="069e4-192">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="069e4-192">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="069e4-194">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="069e4-194">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

    <span data-ttu-id="069e4-196">a.</span><span class="sxs-lookup"><span data-stu-id="069e4-196">a.</span></span> <span data-ttu-id="069e4-197">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="069e4-197">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="069e4-198">b.</span><span class="sxs-lookup"><span data-stu-id="069e4-198">b.</span></span> <span data-ttu-id="069e4-199">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="069e4-199">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="069e4-200">c.</span><span class="sxs-lookup"><span data-stu-id="069e4-200">c.</span></span> <span data-ttu-id="069e4-201">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="069e4-201">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="069e4-202">d.</span><span class="sxs-lookup"><span data-stu-id="069e4-202">d.</span></span> <span data-ttu-id="069e4-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="069e4-203">Click **Create**.</span></span>
 
### <a name="create-a-filecloud-test-user"></a><span data-ttu-id="069e4-204">Vytvoření zkušebního uživatele FileCloud</span><span class="sxs-lookup"><span data-stu-id="069e4-204">Create a FileCloud test user</span></span>

<span data-ttu-id="069e4-205">Hello cílem této části je toocreate volal Britta Simon v FileCloud uživatele.</span><span class="sxs-lookup"><span data-stu-id="069e4-205">hello objective of this section is toocreate a user called Britta Simon in FileCloud.</span></span> <span data-ttu-id="069e4-206">FileCloud podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="069e4-206">FileCloud supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="069e4-207">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="069e4-207">There is no action item for you in this section.</span></span> <span data-ttu-id="069e4-208">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess FileCloud.</span><span class="sxs-lookup"><span data-stu-id="069e4-208">A new user is created during an attempt tooaccess FileCloud if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="069e4-209">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory FileCloud klienta](mailto:support@codelathe.com).</span><span class="sxs-lookup"><span data-stu-id="069e4-209">If you need toocreate a user manually, you need toocontact hello [FileCloud Client support team](mailto:support@codelathe.com).</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="069e4-210">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="069e4-210">Assign hello Azure AD test user</span></span>

<span data-ttu-id="069e4-211">V této části povolíte tak, že udělíte přístup tooFileCloud toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="069e4-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFileCloud.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="069e4-213">**tooassign Britta Simon tooFileCloud, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="069e4-213">**tooassign Britta Simon tooFileCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="069e4-214">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="069e4-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="069e4-216">V seznamu aplikace hello vyberte **FileCloud**.</span><span class="sxs-lookup"><span data-stu-id="069e4-216">In hello applications list, select **FileCloud**.</span></span>

    ![v seznamu aplikace hello Hello FileCloud odkaz](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_app.png)  

3. <span data-ttu-id="069e4-218">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="069e4-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="069e4-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="069e4-220">Click **Add** button.</span></span> <span data-ttu-id="069e4-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="069e4-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="069e4-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="069e4-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="069e4-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="069e4-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="069e4-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="069e4-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="069e4-226">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="069e4-226">Test single sign-on</span></span>

<span data-ttu-id="069e4-227">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="069e4-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="069e4-228">Když kliknete na dlaždici FileCloud hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FileCloud aplikace.</span><span class="sxs-lookup"><span data-stu-id="069e4-228">When you click hello FileCloud tile in hello Access Panel, you should get automatically signed-on tooyour FileCloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="069e4-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="069e4-229">Additional resources</span></span>

* [<span data-ttu-id="069e4-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="069e4-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="069e4-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="069e4-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png

