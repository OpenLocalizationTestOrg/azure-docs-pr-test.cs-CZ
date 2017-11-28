---
title: "Kurz: Azure Active Directory integrace s přední | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a popředí."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 4be363a3d338ec9268f3324daab4a80346ec3131
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a><span data-ttu-id="5cc9b-103">Kurz: Azure Active Directory integrace s popředí</span><span class="sxs-lookup"><span data-stu-id="5cc9b-103">Tutorial: Azure Active Directory integration with Front</span></span>

<span data-ttu-id="5cc9b-104">V tomto kurzu zjistíte, jak toointegrate před službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5cc9b-104">In this tutorial, you learn how toointegrate Front with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5cc9b-105">Integrace přední s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-105">Integrating Front with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5cc9b-106">Můžete ovládat ve službě Azure AD, který má přístup tooFront.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-106">You can control in Azure AD who has access tooFront.</span></span>
- <span data-ttu-id="5cc9b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFront (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-107">You can enable your users tooautomatically get signed-on tooFront (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="5cc9b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="5cc9b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5cc9b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cc9b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cc9b-110">Prerequisites</span></span>

<span data-ttu-id="5cc9b-111">integrace tooconfigure Azure AD s přední, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-111">tooconfigure Azure AD integration with Front, you need hello following items:</span></span>

- <span data-ttu-id="5cc9b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cc9b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5cc9b-113">Popředí jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5cc9b-113">A Front single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5cc9b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5cc9b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5cc9b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5cc9b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5cc9b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5cc9b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5cc9b-118">Scenario description</span></span>
<span data-ttu-id="5cc9b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5cc9b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5cc9b-121">Přidání přední z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5cc9b-121">Adding Front from hello gallery</span></span>
2. <span data-ttu-id="5cc9b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cc9b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-front-from-hello-gallery"></a><span data-ttu-id="5cc9b-123">Přidání přední z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5cc9b-123">Adding Front from hello gallery</span></span>
<span data-ttu-id="5cc9b-124">tooconfigure hello integrace přední do Azure AD, je nutné tooadd přední hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-124">tooconfigure hello integration of Front into Azure AD, you need tooadd Front from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5cc9b-125">**tooadd přední z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cc9b-125">**tooadd Front from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cc9b-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="5cc9b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5cc9b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="5cc9b-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="5cc9b-133">Hello vyhledávacího pole zadejte **Front**, vyberte **Front** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-133">In hello search box, type **Front**, select **Front** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Přední v seznamu výsledků hello](./media/active-directory-saas-front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5cc9b-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cc9b-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="5cc9b-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s přední podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5cc9b-136">In this section, you configure and test Azure AD single sign-on with Front based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5cc9b-137">Pro toowork jeden přihlašování Azure AD musí tooknow vpředu uživatele protějšku hello je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Front is tooa user in Azure AD.</span></span> <span data-ttu-id="5cc9b-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a hello vpředu související uživatelské musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-138">In other words, a link relationship between an Azure AD user and hello related user in Front needs toobe established.</span></span>

<span data-ttu-id="5cc9b-139">Vpředu, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-139">In Front, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5cc9b-140">tooconfigure a testu Azure AD jednotné přihlašování s přední, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-140">tooconfigure and test Azure AD single sign-on with Front, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5cc9b-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5cc9b-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5cc9b-143">**[Vytvoření zkušebního uživatele před](#create-a-front-test-user)**  -toohave Britta Simon protějšku vpředu je reprezentace toohello propojené služby Azure AD uživatele.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-143">**[Create a Front test user](#create-a-front-test-user)** - toohave a counterpart of Britta Simon in Front that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5cc9b-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5cc9b-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5cc9b-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cc9b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5cc9b-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci popředí.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Front application.</span></span>

<span data-ttu-id="5cc9b-148">**tooconfigure Azure AD jednotné přihlašování s přední, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cc9b-148">**tooconfigure Azure AD single sign-on with Front, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cc9b-149">V portálu Azure, na hello hello **Front** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-149">In hello Azure portal, on hello **Front** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="5cc9b-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-front-tutorial/tutorial_front_samlbase.png)

3. <span data-ttu-id="5cc9b-153">Na hello **Front domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-153">On hello **Front Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_url1.png)

    <span data-ttu-id="5cc9b-155">a.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-155">a.</span></span> <span data-ttu-id="5cc9b-156">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="5cc9b-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com`</span></span>

    <span data-ttu-id="5cc9b-157">b.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-157">b.</span></span> <span data-ttu-id="5cc9b-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.frontapp.com/sso/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="5cc9b-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com/sso/saml/callback`</span></span>

4. <span data-ttu-id="5cc9b-159">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-159">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_url2.png)

    <span data-ttu-id="5cc9b-161">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="5cc9b-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="5cc9b-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-162">These values are not real.</span></span> <span data-ttu-id="5cc9b-163">Aktualizace tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL, které jsou vysvětleny později v kurzu nebo kontaktujte [tým podpory Front klienta](mailto:support@frontapp.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL which are explained later in tutorial or contact [Front Client support team](mailto:support@frontapp.com) tooget these values.</span></span> 

5. <span data-ttu-id="5cc9b-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_certificate.png) 

6. <span data-ttu-id="5cc9b-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="5cc9b-168">Na hello **Front konfigurace** klikněte na tlačítko **nakonfigurovat Front** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-168">On hello **Front Configuration** section, click **Configure Front** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5cc9b-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5cc9b-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_configure.png) 

8. <span data-ttu-id="5cc9b-171">Klient přední tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-171">Sign-on tooyour Front tenant as an administrator.</span></span>

9. <span data-ttu-id="5cc9b-172">Přejděte příliš**nastavení (ozubené kolo ikona hello dolnímu okraji levého bočním panelu hello) > Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-172">Go too**Settings (cog icon at hello bottom of hello left sidebar) > Preferences**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

10. <span data-ttu-id="5cc9b-174">Klikněte na tlačítko **jednotné přihlašování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-174">Click **Single Sign On** link.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

11. <span data-ttu-id="5cc9b-176">Vyberte **SAML** v rozevíracím seznamu hello **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-176">Select **SAML** in hello drop-down list of **Single Sign On**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

12. <span data-ttu-id="5cc9b-178">V hello **vstupní bod** textbox put hello hodnotu **jeden přihlašování adresa URL služby** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-178">In hello **Entry Point** textbox put hello value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

13. <span data-ttu-id="5cc9b-180">Otevřete váš stažené **Certificate(Base64)** souborů v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **podpisového certifikátu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-180">Open your downloaded **Certificate(Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Signing certificate** textbox.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

14. <span data-ttu-id="5cc9b-182">Na hello **nastavení poskytovatele služby** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-182">On hello **Service provider settings** section, perform hello following steps:</span></span>

    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

    <span data-ttu-id="5cc9b-184">a.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-184">a.</span></span> <span data-ttu-id="5cc9b-185">Zkopírujte hodnotu hello **Entity ID** a vložte jej do hello **identifikátor** textového pole v **Front domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-185">Copy hello value of **Entity ID** and paste it into hello **Identifier** textbox in **Front Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="5cc9b-186">b.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-186">b.</span></span> <span data-ttu-id="5cc9b-187">Zkopírujte hodnotu hello **adresa URL služby ACS** a vložte jej do hello **přihlašovací adresa URL** textového pole v **Front domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-187">Copy hello value of **ACS URL** and paste it into hello **Sign-on URL** textbox in **Front Domain and URLs** section in Azure portal.</span></span>
    
15. <span data-ttu-id="5cc9b-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="5cc9b-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5cc9b-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5cc9b-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5cc9b-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5cc9b-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5cc9b-192">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cc9b-192">Create an Azure AD test user</span></span>

<span data-ttu-id="5cc9b-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="5cc9b-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cc9b-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cc9b-196">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-196">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-front-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="5cc9b-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-198">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-front-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="5cc9b-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-200">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="5cc9b-202">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5cc9b-202">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

    <span data-ttu-id="5cc9b-204">a.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-204">a.</span></span> <span data-ttu-id="5cc9b-205">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-205">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5cc9b-206">b.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-206">b.</span></span> <span data-ttu-id="5cc9b-207">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-207">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="5cc9b-208">c.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-208">c.</span></span> <span data-ttu-id="5cc9b-209">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-209">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="5cc9b-210">d.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-210">d.</span></span> <span data-ttu-id="5cc9b-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-211">Click **Create**.</span></span>
 
### <a name="create-a-front-test-user"></a><span data-ttu-id="5cc9b-212">Vytvoření zkušebního uživatele před</span><span class="sxs-lookup"><span data-stu-id="5cc9b-212">Create a Front test user</span></span>

<span data-ttu-id="5cc9b-213">V této části vytvoříte uživatele volat Britta Simon vpředu.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-213">In this section, you create a user called Britta Simon in Front.</span></span> <span data-ttu-id="5cc9b-214">Práce s [tým podpory Front klienta](mailto:support@frontapp.com) pro přidání uživatelů hello hello přední platformy.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-214">Work with [Front Client support team](mailto:support@frontapp.com) to add hello users in hello Front platform.</span></span> <span data-ttu-id="5cc9b-215">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-215">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5cc9b-216">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5cc9b-216">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5cc9b-217">V této části povolíte tak, že udělíte přístup tooFront toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFront.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="5cc9b-219">**tooassign Britta Simon tooFront, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cc9b-219">**tooassign Britta Simon tooFront, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cc9b-220">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5cc9b-222">V seznamu aplikace hello vyberte **Front**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-222">In hello applications list, select **Front**.</span></span>

    ![Hello přední odkaz v seznamu aplikace hello](./media/active-directory-saas-front-tutorial/tutorial_front_app.png)  

3. <span data-ttu-id="5cc9b-224">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="5cc9b-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-226">Click **Add** button.</span></span> <span data-ttu-id="5cc9b-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="5cc9b-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5cc9b-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5cc9b-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5cc9b-232">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cc9b-232">Test single sign-on</span></span>

<span data-ttu-id="5cc9b-233">Hello cílem této části je tootest váš pomocí Azure AD SSOconfiguration hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-233">hello objective of this section is tootest your Azure AD SSOconfiguration using hello Access Panel.</span></span>

<span data-ttu-id="5cc9b-234">Když kliknete na dlaždici přední hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour před aplikací.</span><span class="sxs-lookup"><span data-stu-id="5cc9b-234">When you click hello Front tile in hello Access Panel, you should get automatically signed-on tooyour Front application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5cc9b-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5cc9b-235">Additional resources</span></span>

* [<span data-ttu-id="5cc9b-236">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5cc9b-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5cc9b-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5cc9b-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-front-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png

