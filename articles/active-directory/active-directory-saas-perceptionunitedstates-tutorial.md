---
title: "Kurz: Azure Active Directory integrace s dojem Spojených států (Non-UltiPro) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a dojem USA (Non-UltiPro)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 874b5da277b9c68504c4af2ac87ed90d2bbd93b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a><span data-ttu-id="c5194-103">Kurz: Azure Active Directory integrace s dojem Spojených států (Non-UltiPro)</span><span class="sxs-lookup"><span data-stu-id="c5194-103">Tutorial: Azure Active Directory integration with Perception United States (Non-UltiPro)</span></span>

<span data-ttu-id="c5194-104">V tomto kurzu zjistíte, jak toointegrate dojem Spojených států (Non-UltiPro) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c5194-104">In this tutorial, you learn how toointegrate Perception United States (Non-UltiPro) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c5194-105">Integrace dojem USA (Non-UltiPro) s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c5194-105">Integrating Perception United States (Non-UltiPro) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c5194-106">Můžete ovládat ve službě Azure AD, který má přístup tooPerception USA (Non-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="c5194-106">You can control in Azure AD who has access tooPerception United States (Non-UltiPro).</span></span>
- <span data-ttu-id="c5194-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPerception USA (Non-UltiPro) (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c5194-107">You can enable your users tooautomatically get signed-on tooPerception United States (Non-UltiPro) (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c5194-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c5194-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="c5194-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c5194-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5194-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c5194-110">Prerequisites</span></span>

<span data-ttu-id="c5194-111">tooconfigure integrace Azure AD s dojem Spojených států (Non-UltiPro), je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c5194-111">tooconfigure Azure AD integration with Perception United States (Non-UltiPro), you need hello following items:</span></span>

- <span data-ttu-id="c5194-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c5194-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c5194-113">Dojem USA (Non-UltiPro) jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c5194-113">A Perception United States (Non-UltiPro) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c5194-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5194-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c5194-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c5194-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c5194-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c5194-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c5194-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c5194-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c5194-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c5194-118">Scenario description</span></span>
<span data-ttu-id="c5194-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5194-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c5194-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c5194-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c5194-121">Přidání dojem USA (Non-UltiPro) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c5194-121">Adding Perception United States (Non-UltiPro) from hello gallery</span></span>
2. <span data-ttu-id="c5194-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5194-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-perception-united-states-non-ultipro-from-hello-gallery"></a><span data-ttu-id="c5194-123">Přidání dojem USA (Non-UltiPro) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c5194-123">Adding Perception United States (Non-UltiPro) from hello gallery</span></span>
<span data-ttu-id="c5194-124">tooconfigure hello integrace z dojem Spojených států (Non-UltiPro) do Azure AD, musíte tooadd dojem USA (Non-UltiPro) hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c5194-124">tooconfigure hello integration of Perception United States (Non-UltiPro) into Azure AD, you need tooadd Perception United States (Non-UltiPro) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c5194-125">**tooadd dojem Spojených států (Non-UltiPro) z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5194-125">**tooadd Perception United States (Non-UltiPro) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c5194-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c5194-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="c5194-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c5194-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c5194-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c5194-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="c5194-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5194-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="c5194-133">Hello vyhledávacího pole zadejte **dojem USA (Non-UltiPro)**, vyberte **dojem USA (Non-UltiPro)** z panelu výsledků klikněte **přidat** tooadd tlačítko aplikace Hello.</span><span class="sxs-lookup"><span data-stu-id="c5194-133">In hello search box, type **Perception United States (Non-UltiPro)**, select **Perception United States (Non-UltiPro)** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Dojem Spojených států (Non-UltiPro) v seznamu výsledků hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c5194-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5194-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c5194-136">V této části konfiguraci a testování Azure AD jednotné přihlašování s dojem USA (Non-UltiPro) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c5194-136">In this section, you configure and test Azure AD single sign-on with Perception United States (Non-UltiPro) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c5194-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v dojem Spojených států (Non-UltiPro) je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c5194-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Perception United States (Non-UltiPro) is tooa user in Azure AD.</span></span> <span data-ttu-id="c5194-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v dojem Spojených států (Non-UltiPro) musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c5194-138">In other words, a link relationship between an Azure AD user and hello related user in Perception United States (Non-UltiPro) needs toobe established.</span></span>

<span data-ttu-id="c5194-139">V dojem USA (Non-UltiPro), přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c5194-139">In Perception United States (Non-UltiPro), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c5194-140">tooconfigure a testování Azure AD jednotné přihlašování s dojem Spojených států (Non-UltiPro), je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="c5194-140">tooconfigure and test Azure AD single sign-on with Perception United States (Non-UltiPro), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c5194-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c5194-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c5194-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c5194-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c5194-143">**[Vytvoření zkušebního uživatele dojem USA (Non-UltiPro)](#create-a-perception-united-states-non-ultipro-test-user)**  -toohave protějšek Britta Simon v dojem Spojené státy americké (Non-UltiPro), je toohello propojené služby Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c5194-143">**[Create a Perception United States (Non-UltiPro) test user](#create-a-perception-united-states-non-ultipro-test-user)** - toohave a counterpart of Britta Simon in Perception United States (Non-UltiPro) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c5194-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c5194-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c5194-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c5194-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c5194-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5194-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c5194-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci dojem USA (Non-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="c5194-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Perception United States (Non-UltiPro) application.</span></span>

<span data-ttu-id="c5194-148">**tooconfigure Azure AD jednotné přihlašování s dojem Spojených států (Non-UltiPro), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5194-148">**tooconfigure Azure AD single sign-on with Perception United States (Non-UltiPro), perform hello following steps:**</span></span>

1. <span data-ttu-id="c5194-149">V portálu Azure, na hello hello **dojem USA (Non-UltiPro)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c5194-149">In hello Azure portal, on hello **Perception United States (Non-UltiPro)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="c5194-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c5194-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. <span data-ttu-id="c5194-153">Na hello **doméně dojem USA (Non-UltiPro) a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c5194-153">On hello **Perception United States (Non-UltiPro) Domain and URLs** section, perform hello following steps:</span></span>

    ![Dojem USA (Non-UltiPro) domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    <span data-ttu-id="c5194-155">a.</span><span class="sxs-lookup"><span data-stu-id="c5194-155">a.</span></span> <span data-ttu-id="c5194-156">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://perception.kanjoya.com/sp`</span><span class="sxs-lookup"><span data-stu-id="c5194-156">In hello **Identifier** textbox, type hello URL: `https://perception.kanjoya.com/sp`</span></span>

    <span data-ttu-id="c5194-157">b.</span><span class="sxs-lookup"><span data-stu-id="c5194-157">b.</span></span> <span data-ttu-id="c5194-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://perception.kanjoya.com/sso?idp=<entity_id>`</span><span class="sxs-lookup"><span data-stu-id="c5194-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://perception.kanjoya.com/sso?idp=<entity_id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c5194-159">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="c5194-159">hello value is not real.</span></span> <span data-ttu-id="c5194-160">Aktualizujte hodnotu hello s hello skutečná adresa URL odpovědi, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="c5194-160">You will update hello value with hello actual Reply URL, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="c5194-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c5194-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. <span data-ttu-id="c5194-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5194-163">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c5194-165">Na hello **konfigurace dojem USA (Non-UltiPro)** klikněte na tlačítko **konfigurace dojem Spojených států (Non-UltiPro)** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c5194-165">On hello **Perception United States (Non-UltiPro) Configuration** section, click **Configure Perception United States (Non-UltiPro)** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c5194-166">Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c5194-166">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="c5194-167">a.</span><span class="sxs-lookup"><span data-stu-id="c5194-167">a.</span></span> <span data-ttu-id="c5194-168">Hello **dojem USA (Non-UltiPro)** aplikace vyžaduje hello **SAML Entity ID** kódovaný identifikátor uri toobe hodnotu, kterou jste zkopírovali,.</span><span class="sxs-lookup"><span data-stu-id="c5194-168">hello **Perception United States (Non-UltiPro)** application requires hello **SAML Entity ID** value, which you have copied, toobe uri encoded.</span></span> <span data-ttu-id="c5194-169">Hodnota uri kódovaný hello tooget, použijte hello následující odkaz:**http://www.url-encode-decode.com/**.</span><span class="sxs-lookup"><span data-stu-id="c5194-169">tooget hello uri encoded value, use hello following link:**http://www.url-encode-decode.com/**.</span></span>

    <span data-ttu-id="c5194-170">b.</span><span class="sxs-lookup"><span data-stu-id="c5194-170">b.</span></span> <span data-ttu-id="c5194-171">Po získání hello uri šifrovanou hodnotu ho spojovat se hello **adresa URL odpovědi** jak je uvedeno níže -</span><span class="sxs-lookup"><span data-stu-id="c5194-171">After getting hello uri encoded value combine it with hello **Reply URL** as mentioned below-</span></span>

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    <span data-ttu-id="c5194-172">c.</span><span class="sxs-lookup"><span data-stu-id="c5194-172">c.</span></span> <span data-ttu-id="c5194-173">Vložení hello větší než hodnota v hello **adresa URL odpovědi** textového pole v **doméně dojem USA (Non-UltiPro) a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="c5194-173">Paste hello above value in hello **Reply URL** textbox in **Perception United States (Non-UltiPro) Domain and URLs** section.</span></span>

    ![Konfigurace dojem USA (bez UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. <span data-ttu-id="c5194-175">V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti tooyour dojem USA (Non-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="c5194-175">In another browser window, sign on tooyour Perception United States (Non-UltiPro) company site as an administrator.</span></span>

8. <span data-ttu-id="c5194-176">Na hlavním panelu nástrojů hello klikněte na **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="c5194-176">In hello main toolbar, click **Account Settings**.</span></span>

    ![Dojem uživatele USA (Non-UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. <span data-ttu-id="c5194-178">Na hello **nastavení účtu** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c5194-178">On hello **Account Settings** page, perform hello following steps:</span></span>

    ![Dojem uživatele USA (Non-UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    <span data-ttu-id="c5194-180">a.</span><span class="sxs-lookup"><span data-stu-id="c5194-180">a.</span></span> <span data-ttu-id="c5194-181">V hello **název společnosti** textovému poli, název typu hello hello **společnosti**.</span><span class="sxs-lookup"><span data-stu-id="c5194-181">In hello **Company Name** textbox, type hello name of hello **Company**.</span></span>
    
    <span data-ttu-id="c5194-182">b.</span><span class="sxs-lookup"><span data-stu-id="c5194-182">b.</span></span> <span data-ttu-id="c5194-183">V hello **název účtu** textovému poli, název typu hello hello **účet**.</span><span class="sxs-lookup"><span data-stu-id="c5194-183">In hello **Account Name** textbox, type hello name of hello **Account**.</span></span>

    <span data-ttu-id="c5194-184">c.</span><span class="sxs-lookup"><span data-stu-id="c5194-184">c.</span></span> <span data-ttu-id="c5194-185">V **výchozí odpovědi tooEmail** textového pole, platný typ hello **e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="c5194-185">In **Default Reply-tooEmail** text box, type hello valid **Email**.</span></span>

    <span data-ttu-id="c5194-186">d.</span><span class="sxs-lookup"><span data-stu-id="c5194-186">d.</span></span> <span data-ttu-id="c5194-187">Vyberte **zprostředkovatele Identity jednotného přihlašování k** jako **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="c5194-187">Select **SSO Identity Provider** as **SAML 2.0**.</span></span>

10. <span data-ttu-id="c5194-188">Na hello **Konfigurace jednotného přihlašování k** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c5194-188">On hello **SSO Configuration** page, perform hello following steps:</span></span>

    ![SSOConfig dojem USA (bez UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    <span data-ttu-id="c5194-190">a.</span><span class="sxs-lookup"><span data-stu-id="c5194-190">a.</span></span> <span data-ttu-id="c5194-191">Vyberte **SAML NameID typ** jako **e-MAILU**.</span><span class="sxs-lookup"><span data-stu-id="c5194-191">Select **SAML NameID Type** as **EMAIL**.</span></span>

    <span data-ttu-id="c5194-192">b.</span><span class="sxs-lookup"><span data-stu-id="c5194-192">b.</span></span> <span data-ttu-id="c5194-193">V hello **název konfigurace jednotného přihlašování k** textovému poli, název typu hello vaše **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="c5194-193">In hello **SSO Configuration Name** textbox, type hello name of your **Configuration**.</span></span>
    
    <span data-ttu-id="c5194-194">c.</span><span class="sxs-lookup"><span data-stu-id="c5194-194">c.</span></span> <span data-ttu-id="c5194-195">V **název zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c5194-195">In **Identity Provider Name** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="c5194-196">d.</span><span class="sxs-lookup"><span data-stu-id="c5194-196">d.</span></span> <span data-ttu-id="c5194-197">V **SAML domény textbox**, zadejte hello domény jako  **@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="c5194-197">In **SAML Domain textbox**, enter hello domain like **@contoso.com**.</span></span>

    <span data-ttu-id="c5194-198">e.</span><span class="sxs-lookup"><span data-stu-id="c5194-198">e.</span></span> <span data-ttu-id="c5194-199">Klikněte na **nahrát znovu** tooupload hello **soubor XML s metadaty** souboru.</span><span class="sxs-lookup"><span data-stu-id="c5194-199">Click on **Upload Again** tooupload hello **Metadata XML** file.</span></span>

    <span data-ttu-id="c5194-200">f.</span><span class="sxs-lookup"><span data-stu-id="c5194-200">f.</span></span> <span data-ttu-id="c5194-201">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="c5194-201">Click **Update**.</span></span>


> [!TIP]
> <span data-ttu-id="c5194-202">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c5194-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c5194-203">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c5194-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c5194-204">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c5194-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c5194-205">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c5194-205">Create an Azure AD test user</span></span>

<span data-ttu-id="c5194-206">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c5194-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="c5194-208">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5194-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c5194-209">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5194-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c5194-211">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c5194-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c5194-213">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c5194-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c5194-215">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c5194-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c5194-217">a.</span><span class="sxs-lookup"><span data-stu-id="c5194-217">a.</span></span> <span data-ttu-id="c5194-218">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c5194-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c5194-219">b.</span><span class="sxs-lookup"><span data-stu-id="c5194-219">b.</span></span> <span data-ttu-id="c5194-220">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c5194-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="c5194-221">c.</span><span class="sxs-lookup"><span data-stu-id="c5194-221">c.</span></span> <span data-ttu-id="c5194-222">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="c5194-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="c5194-223">d.</span><span class="sxs-lookup"><span data-stu-id="c5194-223">d.</span></span> <span data-ttu-id="c5194-224">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c5194-224">Click **Create**.</span></span>
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a><span data-ttu-id="c5194-225">Vytvoření zkušebního uživatele dojem USA (Non-UltiPro)</span><span class="sxs-lookup"><span data-stu-id="c5194-225">Create a Perception United States (Non-UltiPro) test user</span></span>

<span data-ttu-id="c5194-226">V této části vytvoříte uživatele volat Britta Simon v dojem Spojených států (Non-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="c5194-226">In this section, you create a user called Britta Simon in Perception United States (Non-UltiPro).</span></span> <span data-ttu-id="c5194-227">Práce s [tým podpory dojem USA (Non-UltiPro)](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello uživatelé v platformě hello dojem USA (Non-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="c5194-227">Work with [Perception United States (Non-UltiPro) support team](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello users in hello Perception United States (Non-UltiPro) platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c5194-228">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c5194-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c5194-229">V této části povolíte tak, že udělíte přístup tooPerception USA (Non-UltiPro) toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c5194-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPerception United States (Non-UltiPro).</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="c5194-231">**tooassign Britta Simon tooPerception USA (Non-UltiPro), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c5194-231">**tooassign Britta Simon tooPerception United States (Non-UltiPro), perform hello following steps:**</span></span>

1. <span data-ttu-id="c5194-232">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c5194-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c5194-234">V seznamu aplikace hello vyberte **dojem USA (Non-UltiPro)**.</span><span class="sxs-lookup"><span data-stu-id="c5194-234">In hello applications list, select **Perception United States (Non-UltiPro)**.</span></span>

    ![odkaz Hello dojem USA (Non-UltiPro) v seznamu aplikace hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. <span data-ttu-id="c5194-236">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c5194-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="c5194-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c5194-238">Click **Add** button.</span></span> <span data-ttu-id="c5194-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c5194-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="c5194-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c5194-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c5194-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c5194-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c5194-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c5194-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c5194-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c5194-244">Test single sign-on</span></span>

<span data-ttu-id="c5194-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c5194-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c5194-246">Když kliknete na dlaždici hello dojem USA (Non-UltiPro) v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour dojem USA (Non-UltiPro) aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5194-246">When you click hello Perception United States (Non-UltiPro) tile in hello Access Panel, you should get automatically signed-on tooyour Perception United States (Non-UltiPro) application.</span></span>
<span data-ttu-id="c5194-247">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c5194-247">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c5194-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c5194-248">Additional resources</span></span>

* [<span data-ttu-id="c5194-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c5194-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c5194-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c5194-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

