---
title: 'Kurz: Azure Active Directory integrace s Asana | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Asana."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: 9ac35dedc809b2b3dfb461d92a5afb066ccdedde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a><span data-ttu-id="fb9dc-103">Kurz: Azure Active Directory integrace s Asana</span><span class="sxs-lookup"><span data-stu-id="fb9dc-103">Tutorial: Azure Active Directory integration with Asana</span></span>

<span data-ttu-id="fb9dc-104">V tomto kurzu zjistíte, jak toointegrate Asana s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fb9dc-104">In this tutorial, you learn how toointegrate Asana with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fb9dc-105">Integrace Asana s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-105">Integrating Asana with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fb9dc-106">Můžete řídit ve službě Azure AD, který má přístup tooAsana</span><span class="sxs-lookup"><span data-stu-id="fb9dc-106">You can control in Azure AD who has access tooAsana</span></span>
- <span data-ttu-id="fb9dc-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAsana (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb9dc-107">You can enable your users tooautomatically get signed-on tooAsana (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fb9dc-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fb9dc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="fb9dc-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fb9dc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb9dc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fb9dc-110">Prerequisites</span></span>

<span data-ttu-id="fb9dc-111">Integrace služby Azure AD s Asana tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-111">tooconfigure Azure AD integration with Asana, you need hello following items:</span></span>

- <span data-ttu-id="fb9dc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb9dc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fb9dc-113">Asana jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="fb9dc-113">An Asana single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fb9dc-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fb9dc-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fb9dc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fb9dc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fb9dc-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fb9dc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="fb9dc-118">Scenario description</span></span>
<span data-ttu-id="fb9dc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fb9dc-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fb9dc-121">Přidání Asana z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="fb9dc-121">Adding Asana from hello gallery</span></span>
2. <span data-ttu-id="fb9dc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb9dc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asana-from-hello-gallery"></a><span data-ttu-id="fb9dc-123">Přidání Asana z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="fb9dc-123">Adding Asana from hello gallery</span></span>
<span data-ttu-id="fb9dc-124">tooconfigure hello integrace Asana do Azure AD, je nutné tooadd Asana hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-124">tooconfigure hello integration of Asana into Azure AD, you need tooadd Asana from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fb9dc-125">**tooadd Asana z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fb9dc-125">**tooadd Asana from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9dc-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="fb9dc-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fb9dc-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="fb9dc-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="fb9dc-133">Hello vyhledávacího pole zadejte **Asana**, vyberte **Asana** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-133">In hello search box, type **Asana**, select **Asana** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fb9dc-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb9dc-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="fb9dc-136">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Asana podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="fb9dc-136">In this section, you configure and test Azure AD single sign-on with Asana based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fb9dc-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Asana je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Asana is tooa user in Azure AD.</span></span> <span data-ttu-id="fb9dc-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Asana musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-138">In other words, a link relationship between an Azure AD user and hello related user in Asana needs toobe established.</span></span>

<span data-ttu-id="fb9dc-139">V Asana, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-139">In Asana, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fb9dc-140">tooconfigure a testu Azure AD jednotné přihlašování s Asana, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-140">tooconfigure and test Azure AD single sign-on with Asana, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fb9dc-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fb9dc-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fb9dc-143">**[Vytvořit testovací uživatele s Asana](#create-an-asana-test-user)**  -toohave protějšek Britta Simon v Asana, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-143">**[Create an Asana test user](#create-an-asana-test-user)** - toohave a counterpart of Britta Simon in Asana that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fb9dc-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fb9dc-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fb9dc-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb9dc-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fb9dc-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Asana.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Asana application.</span></span>

<span data-ttu-id="fb9dc-148">**tooconfigure Azure AD jednotné přihlašování s Asana, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fb9dc-148">**tooconfigure Azure AD single sign-on with Asana, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9dc-149">V portálu Azure, na hello hello **Asana** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-149">In hello Azure portal, on hello **Asana** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="fb9dc-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. <span data-ttu-id="fb9dc-153">Na hello **Asana domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-153">On hello **Asana Domain and URLs** section, perform hello following steps:</span></span>

    ![Asana domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    <span data-ttu-id="fb9dc-155">a.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-155">a.</span></span> <span data-ttu-id="fb9dc-156">V hello **přihlašovací adresa URL** textovému poli, zadat adresu URL:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="fb9dc-156">In hello **Sign-on URL** textbox, type URL: `https://app.asana.com/`</span></span>

    <span data-ttu-id="fb9dc-157">b.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-157">b.</span></span> <span data-ttu-id="fb9dc-158">V hello **identifikátor** textovému poli, hodnota typu:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="fb9dc-158">In hello **Identifier** textbox, type value: `https://app.asana.com/`</span></span>
 
4. <span data-ttu-id="fb9dc-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. <span data-ttu-id="fb9dc-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fb9dc-163">Na hello **Asana konfigurace** klikněte na tlačítko **konfigurace Asana** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-163">On hello **Asana Configuration** section, click **Configure Asana** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fb9dc-164">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="fb9dc-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Asana](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. <span data-ttu-id="fb9dc-166">V okně jiný prohlížeč, přihlášení tooyour Asana aplikace.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-166">In a different browser window, sign-on tooyour Asana application.</span></span> <span data-ttu-id="fb9dc-167">tooconfigure jednotné přihlašování v Asana, nastavení pracovního prostoru hello přístup kliknutím na název pracovního prostoru hello na hello pravém horním rohu úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-167">tooconfigure SSO in Asana, access hello workspace settings by clicking hello workspace name on hello top right corner of hello screen.</span></span> <span data-ttu-id="fb9dc-168">Potom klikněte na  **\<název pracovního prostoru\> nastavení**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-168">Then, click on **\<your workspace name\> Settings**.</span></span> 
   
    ![Nastavení jednotného přihlašování k Asana](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. <span data-ttu-id="fb9dc-170">Na hello **nastavení organizace** okně klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-170">On hello **Organization settings** window, click **Administration**.</span></span> <span data-ttu-id="fb9dc-171">Potom klikněte na **členy musí přihlásit pomocí SAML** tooenable hello jednotné přihlašování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-171">Then, click **Members must log in via SAML** tooenable hello SSO configuration.</span></span> <span data-ttu-id="fb9dc-172">Hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-172">hello perform hello following steps:</span></span>
   
    ![Konfigurace nastavení jednotný přihlášení](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     <span data-ttu-id="fb9dc-174">a.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-174">a.</span></span> <span data-ttu-id="fb9dc-175">V hello **přihlašovací adresa URL stránky** textovému poli, vložte hello **SAML jeden přihlašování adresa URL služby**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-175">In hello **Sign-in page URL** textbox, paste hello **SAML Single Sign-On Service URL**.</span></span>

     <span data-ttu-id="fb9dc-176">b.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-176">b.</span></span> <span data-ttu-id="fb9dc-177">Klikněte pravým tlačítkem na certifikát hello si stáhli z portálu Azure a pak otevřete soubor certifikátu hello pomocí poznámkového bloku nebo upřednostňovaný textový editor.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-177">Right click hello certificate downloaded from Azure portal, then open hello certificate file using Notepad or your preferred text editor.</span></span> <span data-ttu-id="fb9dc-178">Kopie hello obsahu mezi hello začít a hello název certifikátu koncovým a vložte ji do hello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-178">Copy hello content between hello begin and hello end certificate title and paste it in hello **X.509 Certificate** textbox.</span></span>

9. <span data-ttu-id="fb9dc-179">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-179">Click **Save**.</span></span> <span data-ttu-id="fb9dc-180">Přejděte příliš[Asana Průvodce pro nastavení jednotného přihlašování k](https://asana.com/guide/help/premium/authentication#gl-saml) Pokud potřebujete další pomoc.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-180">Go too[Asana guide for setting up SSO](https://asana.com/guide/help/premium/authentication#gl-saml) if you need further assistance.</span></span>

> [!TIP]
> <span data-ttu-id="fb9dc-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="fb9dc-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fb9dc-182">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fb9dc-183">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fb9dc-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fb9dc-184">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb9dc-184">Create an Azure AD test user</span></span>

<span data-ttu-id="fb9dc-185">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="fb9dc-187">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fb9dc-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9dc-188">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fb9dc-190">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fb9dc-192">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fb9dc-194">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fb9dc-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fb9dc-196">a.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-196">a.</span></span> <span data-ttu-id="fb9dc-197">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fb9dc-198">b.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-198">b.</span></span> <span data-ttu-id="fb9dc-199">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fb9dc-200">c.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-200">c.</span></span> <span data-ttu-id="fb9dc-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="fb9dc-202">d.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-202">d.</span></span> <span data-ttu-id="fb9dc-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-203">Click **Create**.</span></span>
 
### <a name="create-an-asana-test-user"></a><span data-ttu-id="fb9dc-204">Vytvořit uživatele s Asana testu</span><span class="sxs-lookup"><span data-stu-id="fb9dc-204">Create an Asana test user</span></span>

<span data-ttu-id="fb9dc-205">V této části vytvoříte volal Britta Simon v Asana uživatele.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-205">In this section, you create a user called Britta Simon in Asana.</span></span>

1. <span data-ttu-id="fb9dc-206">Na **Asana**, přejděte toohello **týmy** části na levém panelu hello.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-206">On **Asana**, go toohello **Teams** section on hello left panel.</span></span> <span data-ttu-id="fb9dc-207">Klikněte na tlačítko hello plus tlačítko přihlásit.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-207">Click hello plus sign button.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. <span data-ttu-id="fb9dc-209">Zadejte e-mailu hello britta.simon@contoso.com v hello textového pole a pak vyberte **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-209">Type hello email britta.simon@contoso.com in hello text box and then select **Invite**.</span></span>

3. <span data-ttu-id="fb9dc-210">Klikněte na tlačítko **odeslat pozvání**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-210">Click **Send Invite**.</span></span> <span data-ttu-id="fb9dc-211">Nový uživatel Hello obdrží e-mail do své e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-211">hello new user will receive an email into her email account.</span></span> <span data-ttu-id="fb9dc-212">Uživatel bude muset toocreate a ověření účtu hello.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-212">She will need toocreate and validate hello account.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="fb9dc-213">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="fb9dc-213">Assign hello Azure AD test user</span></span>

<span data-ttu-id="fb9dc-214">V této části povolíte tak, že udělíte přístup tooAsana toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAsana.</span></span>

![Přiřadit role uživatele hello][200]

<span data-ttu-id="fb9dc-216">**tooassign Britta Simon tooAsana, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="fb9dc-216">**tooassign Britta Simon tooAsana, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9dc-217">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="fb9dc-219">V seznamu aplikace hello vyberte **Asana**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-219">In hello applications list, select **Asana**.</span></span>

    ![v seznamu aplikace hello Hello Asana odkaz](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. <span data-ttu-id="fb9dc-221">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="fb9dc-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-223">Click **Add** button.</span></span> <span data-ttu-id="fb9dc-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="fb9dc-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fb9dc-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fb9dc-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fb9dc-229">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="fb9dc-229">Test single sign-on</span></span>

<span data-ttu-id="fb9dc-230">Hello cílem této části je tootest vaší služby Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-230">hello objective of this section is tootest your Azure AD single sign-on.</span></span>

<span data-ttu-id="fb9dc-231">Přejděte tooAsana přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-231">Go tooAsana login page.</span></span> <span data-ttu-id="fb9dc-232">V textovém poli hello e-mailovou adresu vložit hello e-mailovou adresu britta.simon@contoso.com. Ponechte textové hello hesla v prázdné a pak klikněte na **protokolu v**.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-232">In hello Email address textbox, insert hello email address britta.simon@contoso.com. Leave hello password textbox in blank and then click **Log In**.</span></span> <span data-ttu-id="fb9dc-233">Bude přesměrované tooAzure AD přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-233">You will be redirected tooAzure AD login page.</span></span> <span data-ttu-id="fb9dc-234">Dokončení přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-234">Complete your Azure AD credentials.</span></span> <span data-ttu-id="fb9dc-235">Nyní jste přihlášeni Asana.</span><span class="sxs-lookup"><span data-stu-id="fb9dc-235">Now, you are logged in on Asana.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb9dc-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fb9dc-236">Additional resources</span></span>

* [<span data-ttu-id="fb9dc-237">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fb9dc-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fb9dc-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fb9dc-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
