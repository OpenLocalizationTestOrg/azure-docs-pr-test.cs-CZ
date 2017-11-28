---
title: 'Kurz: Azure Active Directory integrace s technologie Rally softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a technologie Rally softwaru."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: c75c8b98ce7fab19964c13de5ad7e19ef3ebd0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a><span data-ttu-id="5f299-103">Kurz: Azure Active Directory integrace s technologie Rally softwaru</span><span class="sxs-lookup"><span data-stu-id="5f299-103">Tutorial: Azure Active Directory integration with Rally Software</span></span>

<span data-ttu-id="5f299-104">V tomto kurzu zjistíte, jak toointegrate technologie Rally Software s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5f299-104">In this tutorial, you learn how toointegrate Rally Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5f299-105">Integrace technologie Rally softwaru s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5f299-105">Integrating Rally Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5f299-106">Můžete ovládat ve službě Azure AD, který má přístup tooRally softwaru.</span><span class="sxs-lookup"><span data-stu-id="5f299-106">You can control in Azure AD who has access tooRally Software.</span></span>
- <span data-ttu-id="5f299-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRally softwaru (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f299-107">You can enable your users tooautomatically get signed-on tooRally Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="5f299-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f299-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="5f299-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5f299-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f299-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f299-110">Prerequisites</span></span>

<span data-ttu-id="5f299-111">tooconfigure integrace Azure AD s technologie Rally softwaru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5f299-111">tooconfigure Azure AD integration with Rally Software, you need hello following items:</span></span>

- <span data-ttu-id="5f299-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f299-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5f299-113">Technologie Rally softwaru jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5f299-113">A Rally Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5f299-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f299-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5f299-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5f299-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5f299-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5f299-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5f299-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f299-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5f299-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5f299-118">Scenario description</span></span>
<span data-ttu-id="5f299-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f299-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5f299-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5f299-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5f299-121">Přidání technologie Rally softwaru z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5f299-121">Adding Rally Software from hello gallery</span></span>
2. <span data-ttu-id="5f299-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f299-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rally-software-from-hello-gallery"></a><span data-ttu-id="5f299-123">Přidání technologie Rally softwaru z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5f299-123">Adding Rally Software from hello gallery</span></span>
<span data-ttu-id="5f299-124">tooconfigure hello integrace technologie Rally softwaru do služby Azure AD, je nutné tooadd technologie Rally softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5f299-124">tooconfigure hello integration of Rally Software into Azure AD, you need tooadd Rally Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5f299-125">**tooadd technologie Rally softwaru z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f299-125">**tooadd Rally Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f299-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5f299-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="5f299-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5f299-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5f299-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f299-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="5f299-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f299-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="5f299-133">Hello vyhledávacího pole zadejte **technologie Rally softwaru**, vyberte **technologie Rally softwaru** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f299-133">In hello search box, type **Rally Software**, select **Rally Software** from result panel then click **Add** button tooadd hello application.</span></span>

    ![V seznamu výsledků hello technologie Rally softwaru](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5f299-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f299-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="5f299-136">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí technologie Rally Software založený na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5f299-136">In this section, you configure and test Azure AD single sign-on with Rally Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5f299-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v technologie Rally softwaru je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f299-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Rally Software is tooa user in Azure AD.</span></span> <span data-ttu-id="5f299-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru technologie Rally musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5f299-138">In other words, a link relationship between an Azure AD user and hello related user in Rally Software needs toobe established.</span></span>

<span data-ttu-id="5f299-139">V softwaru technologie Rally přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5f299-139">In Rally Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5f299-140">tooconfigure a testu Azure AD jednotné přihlašování s technologie Rally softwaru, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="5f299-140">tooconfigure and test Azure AD single sign-on with Rally Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5f299-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5f299-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5f299-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f299-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5f299-143">**[Vytvoření zkušebního uživatele technologie Rally softwaru](#create-a-rally-software-test-user)**  -toohave protějšek Britta Simon v technologie Rally Software, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f299-143">**[Create a Rally Software test user](#create-a-rally-software-test-user)** - toohave a counterpart of Britta Simon in Rally Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5f299-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f299-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5f299-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5f299-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5f299-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f299-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5f299-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci technologie Rally softwaru.</span><span class="sxs-lookup"><span data-stu-id="5f299-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Rally Software application.</span></span>

<span data-ttu-id="5f299-148">**tooconfigure Azure AD jednotné přihlašování s technologie Rally softwarem, provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5f299-148">**tooconfigure Azure AD single sign-on with Rally Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f299-149">V portálu Azure, na hello hello **technologie Rally softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5f299-149">In hello Azure portal, on hello **Rally Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="5f299-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f299-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. <span data-ttu-id="5f299-153">Na hello **technologie Rally softwaru domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5f299-153">On hello **Rally Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Technologie Rally softwaru adresy URL jeden přihlašování informace o doméně a](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    <span data-ttu-id="5f299-155">a.</span><span class="sxs-lookup"><span data-stu-id="5f299-155">a.</span></span> <span data-ttu-id="5f299-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="5f299-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.rally.com`</span></span>

    <span data-ttu-id="5f299-157">b.</span><span class="sxs-lookup"><span data-stu-id="5f299-157">b.</span></span> <span data-ttu-id="5f299-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="5f299-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.rally.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5f299-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5f299-159">These values are not real.</span></span> <span data-ttu-id="5f299-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5f299-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5f299-161">Obraťte se na [tým podpory technologie Rally klientský Software](https://help.rallydev.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5f299-161">Contact [Rally Software Client support team](https://help.rallydev.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="5f299-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5f299-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. <span data-ttu-id="5f299-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f299-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5f299-166">Na hello **technologie Rally softwarové konfigurace** klikněte na tlačítko **konfigurace technologie Rally softwaru** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5f299-166">On hello **Rally Software Configuration** section, click **Configure Rally Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5f299-167">Kopírování hello **Sign-Out adresy URL a SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5f299-167">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Technologie Rally konfigurace softwaru](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. <span data-ttu-id="5f299-169">Přihlaste se tooyour **technologie Rally softwaru** klienta.</span><span class="sxs-lookup"><span data-stu-id="5f299-169">Log in tooyour **Rally Software** tenant.</span></span>

8. <span data-ttu-id="5f299-170">V panelu nástrojů hello hello nahoře, klikněte na **instalace**a potom vyberte **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="5f299-170">In hello toolbar on hello top, click **Setup**, and then select **Subscription**.</span></span>
   
    <span data-ttu-id="5f299-171">![Předplatné](./media/active-directory-saas-rally-software-tutorial/ic769531.png "předplatného")</span><span class="sxs-lookup"><span data-stu-id="5f299-171">![Subscription](./media/active-directory-saas-rally-software-tutorial/ic769531.png "Subscription")</span></span>

9. <span data-ttu-id="5f299-172">Klikněte na tlačítko hello **akce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f299-172">Click hello **Action** button.</span></span> <span data-ttu-id="5f299-173">Vyberte **upravit předplatné** v hello horní pravé části hello nástrojů.</span><span class="sxs-lookup"><span data-stu-id="5f299-173">Select **Edit Subscription** at hello top right side of hello toolbar.</span></span>

10. <span data-ttu-id="5f299-174">Na hello **předplatné** dialogové okno stránky, proveďte následující kroky hello a pak klikněte na **uložit a zavřít**:</span><span class="sxs-lookup"><span data-stu-id="5f299-174">On hello **Subscription** dialog page, perform hello following steps, and then click **Save & Close**:</span></span>
   
    <span data-ttu-id="5f299-175">![Ověřování](./media/active-directory-saas-rally-software-tutorial/ic769542.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="5f299-175">![Authentication](./media/active-directory-saas-rally-software-tutorial/ic769542.png "Authentication")</span></span>
   
    <span data-ttu-id="5f299-176">a.</span><span class="sxs-lookup"><span data-stu-id="5f299-176">a.</span></span> <span data-ttu-id="5f299-177">Vyberte **technologie Rally nebo jednotného přihlašování k ověřování** z rozevíracího seznamu ověřování.</span><span class="sxs-lookup"><span data-stu-id="5f299-177">Select **Rally or SSO authentication** from Authentication dropdown.</span></span>

    <span data-ttu-id="5f299-178">b.</span><span class="sxs-lookup"><span data-stu-id="5f299-178">b.</span></span> <span data-ttu-id="5f299-179">V hello **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f299-179">In hello **Identity provider URL** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="5f299-180">c.</span><span class="sxs-lookup"><span data-stu-id="5f299-180">c.</span></span> <span data-ttu-id="5f299-181">V hello **jednotného přihlašování k odhlášení** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f299-181">In hello **SSO Logout** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="5f299-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5f299-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5f299-183">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5f299-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5f299-184">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5f299-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5f299-185">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f299-185">Create an Azure AD test user</span></span>

<span data-ttu-id="5f299-186">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5f299-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="5f299-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f299-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f299-189">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f299-189">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="5f299-191">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5f299-191">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="5f299-193">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f299-193">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="5f299-195">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5f299-195">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    <span data-ttu-id="5f299-197">a.</span><span class="sxs-lookup"><span data-stu-id="5f299-197">a.</span></span> <span data-ttu-id="5f299-198">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5f299-198">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5f299-199">b.</span><span class="sxs-lookup"><span data-stu-id="5f299-199">b.</span></span> <span data-ttu-id="5f299-200">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f299-200">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="5f299-201">c.</span><span class="sxs-lookup"><span data-stu-id="5f299-201">c.</span></span> <span data-ttu-id="5f299-202">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="5f299-202">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="5f299-203">d.</span><span class="sxs-lookup"><span data-stu-id="5f299-203">d.</span></span> <span data-ttu-id="5f299-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5f299-204">Click **Create**.</span></span>
 
### <a name="create-a-rally-software-test-user"></a><span data-ttu-id="5f299-205">Vytvoření zkušebního uživatele technologie Rally softwaru</span><span class="sxs-lookup"><span data-stu-id="5f299-205">Create a Rally Software test user</span></span>

<span data-ttu-id="5f299-206">Pro Azure AD Uživatelé toobe možné toosign v musí být zřízená toohello technologie Rally softwarová aplikace pomocí jejich názvy uživatele Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5f299-206">For Azure AD users toobe able toosign in, they must be provisioned toohello Rally Software application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="5f299-207">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f299-207">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f299-208">Přihlaste se tooyour technologie Rally softwaru klienta.</span><span class="sxs-lookup"><span data-stu-id="5f299-208">Log in tooyour Rally Software tenant.</span></span>

2. <span data-ttu-id="5f299-209">Přejděte příliš**instalace \> uživatelé**a potom klikněte na **+ přidat nový**.</span><span class="sxs-lookup"><span data-stu-id="5f299-209">Go too**Setup \> USERS**, and then click **+ Add New**.</span></span>
   
    <span data-ttu-id="5f299-210">![Uživatelé](./media/active-directory-saas-rally-software-tutorial/ic781039.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="5f299-210">![Users](./media/active-directory-saas-rally-software-tutorial/ic781039.png "Users")</span></span>

3. <span data-ttu-id="5f299-211">Zadejte název hello v textovém poli hello nového uživatele a pak klikněte na tlačítko **přidat s podrobnostmi**.</span><span class="sxs-lookup"><span data-stu-id="5f299-211">Type hello name in hello New User textbox, and then click **Add with Details**.</span></span>

4. <span data-ttu-id="5f299-212">V hello **vytvořit uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5f299-212">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5f299-213">![Vytvoření uživatele](./media/active-directory-saas-rally-software-tutorial/ic781040.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="5f299-213">![Create User](./media/active-directory-saas-rally-software-tutorial/ic781040.png "Create User")</span></span>

    <span data-ttu-id="5f299-214">a.</span><span class="sxs-lookup"><span data-stu-id="5f299-214">a.</span></span> <span data-ttu-id="5f299-215">V hello **uživatelské jméno** textovému poli, název typu hello uživatele jako **Brittsimon**.</span><span class="sxs-lookup"><span data-stu-id="5f299-215">In hello **User Name** textbox, type hello name of user like **Brittsimon**.</span></span>
   
    <span data-ttu-id="5f299-216">b.</span><span class="sxs-lookup"><span data-stu-id="5f299-216">b.</span></span> <span data-ttu-id="5f299-217">V **e-mailovou adresu** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="5f299-217">In **E-mail Address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="5f299-218">c.</span><span class="sxs-lookup"><span data-stu-id="5f299-218">c.</span></span> <span data-ttu-id="5f299-219">V **křestní jméno** textové pole, zadejte hello křestní jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="5f299-219">In **First Name** text box, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="5f299-220">d.</span><span class="sxs-lookup"><span data-stu-id="5f299-220">d.</span></span> <span data-ttu-id="5f299-221">V **příjmení** text zadejte příjmení uživatele jako hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="5f299-221">In **Last Name** text box, enter hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="5f299-222">e.</span><span class="sxs-lookup"><span data-stu-id="5f299-222">e.</span></span> <span data-ttu-id="5f299-223">Klikněte na tlačítko **uložit a zavřít**.</span><span class="sxs-lookup"><span data-stu-id="5f299-223">Click **Save & Close**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="5f299-224">Můžete použít žádné jiné technologie Rally softwaru uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované technologie Rally softwaru tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f299-224">You can use any other Rally Software user account creation tools or APIs provided by Rally Software tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5f299-225">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5f299-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5f299-226">V této části povolíte tak, že udělíte přístup tooRally softwaru Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f299-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRally Software.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="5f299-228">**tooassign Britta Simon tooRally softwaru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f299-228">**tooassign Britta Simon tooRally Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f299-229">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f299-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5f299-231">V seznamu aplikace hello vyberte **technologie Rally softwaru**.</span><span class="sxs-lookup"><span data-stu-id="5f299-231">In hello applications list, select **Rally Software**.</span></span>

    ![odkaz na Software technologie Rally Hello v seznamu aplikace hello](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. <span data-ttu-id="5f299-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5f299-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="5f299-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f299-235">Click **Add** button.</span></span> <span data-ttu-id="5f299-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f299-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="5f299-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5f299-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5f299-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f299-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5f299-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f299-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5f299-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f299-241">Test single sign-on</span></span>

<span data-ttu-id="5f299-242">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5f299-242">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5f299-243">Když kliknete na dlaždici technologie Rally softwaru hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour technologie Rally softwarová aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f299-243">When you click hello Rally Software tile in hello Access Panel, you should get automatically signed-on tooyour Rally Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f299-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5f299-244">Additional resources</span></span>

* [<span data-ttu-id="5f299-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5f299-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5f299-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5f299-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

