---
title: 'Kurz: Azure Active Directory integrace s Printix | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Printix."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 654810116091eb52912b377cc97afef803ee816e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a><span data-ttu-id="b09e2-103">Kurz: Azure Active Directory integrace s Printix</span><span class="sxs-lookup"><span data-stu-id="b09e2-103">Tutorial: Azure Active Directory integration with Printix</span></span>

<span data-ttu-id="b09e2-104">V tomto kurzu zjistíte, jak toointegrate Printix s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b09e2-104">In this tutorial, you learn how toointegrate Printix with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b09e2-105">Integrace Printix s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b09e2-105">Integrating Printix with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b09e2-106">Můžete řídit ve službě Azure AD, který má přístup tooPrintix</span><span class="sxs-lookup"><span data-stu-id="b09e2-106">You can control in Azure AD who has access tooPrintix</span></span>
- <span data-ttu-id="b09e2-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPrintix (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b09e2-107">You can enable your users tooautomatically get signed-on tooPrintix (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b09e2-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b09e2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b09e2-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b09e2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b09e2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b09e2-110">Prerequisites</span></span>

<span data-ttu-id="b09e2-111">Integrace služby Azure AD s Printix tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b09e2-111">tooconfigure Azure AD integration with Printix, you need hello following items:</span></span>

- <span data-ttu-id="b09e2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b09e2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b09e2-113">Printix jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b09e2-113">A Printix single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b09e2-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b09e2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b09e2-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b09e2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b09e2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b09e2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b09e2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b09e2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b09e2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b09e2-118">Scenario description</span></span>
<span data-ttu-id="b09e2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b09e2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b09e2-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b09e2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b09e2-121">Přidání Printix z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b09e2-121">Adding Printix from hello gallery</span></span>
2. <span data-ttu-id="b09e2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b09e2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-printix-from-hello-gallery"></a><span data-ttu-id="b09e2-123">Přidání Printix z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b09e2-123">Adding Printix from hello gallery</span></span>
<span data-ttu-id="b09e2-124">tooconfigure hello integrace Printix do Azure AD, je nutné tooadd Printix hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b09e2-124">tooconfigure hello integration of Printix into Azure AD, you need tooadd Printix from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b09e2-125">**tooadd Printix z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b09e2-125">**tooadd Printix from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b09e2-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b09e2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b09e2-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b09e2-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b09e2-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b09e2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b09e2-133">Hello vyhledávacího pole zadejte **Printix**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-133">In hello search box, type **Printix**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. <span data-ttu-id="b09e2-135">Na panelu výsledků hello vyberte **Printix**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b09e2-135">In hello results panel, select **Printix**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b09e2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b09e2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b09e2-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Printix podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b09e2-138">In this section, you configure and test Azure AD single sign-on with Printix based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b09e2-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Printix je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b09e2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Printix is tooa user in Azure AD.</span></span> <span data-ttu-id="b09e2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Printix musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b09e2-140">In other words, a link relationship between an Azure AD user and hello related user in Printix needs toobe established.</span></span>

<span data-ttu-id="b09e2-141">V Printix, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b09e2-141">In Printix, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b09e2-142">tooconfigure a testu Azure AD jednotné přihlašování s Printix, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b09e2-142">tooconfigure and test Azure AD single sign-on with Printix, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b09e2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b09e2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b09e2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b09e2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b09e2-145">**[Vytvoření zkušebního uživatele Printix](#creating-a-printix-test-user)**  -toohave protějšek Britta Simon v Printix, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b09e2-145">**[Creating a Printix test user](#creating-a-printix-test-user)** - toohave a counterpart of Britta Simon in Printix that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b09e2-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b09e2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b09e2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b09e2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b09e2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b09e2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b09e2-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Printix.</span><span class="sxs-lookup"><span data-stu-id="b09e2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Printix application.</span></span>

<span data-ttu-id="b09e2-150">**tooconfigure Azure AD jednotné přihlašování s Printix, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b09e2-150">**tooconfigure Azure AD single sign-on with Printix, perform hello following steps:**</span></span>

1. <span data-ttu-id="b09e2-151">V portálu Azure, na hello hello **Printix** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-151">In hello Azure portal, on hello **Printix** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b09e2-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b09e2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. <span data-ttu-id="b09e2-155">Na hello **Printix domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b09e2-155">On hello **Printix Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    <span data-ttu-id="b09e2-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.printix.net`</span><span class="sxs-lookup"><span data-stu-id="b09e2-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.printix.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b09e2-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="b09e2-158">hello value is not real.</span></span> <span data-ttu-id="b09e2-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b09e2-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b09e2-160">Obraťte se na [tým podpory Printix klienta](mailto:support@printix.net) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b09e2-160">Contact [Printix Client support team](mailto:support@printix.net) tooget hello value.</span></span> 
 
4. <span data-ttu-id="b09e2-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b09e2-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. <span data-ttu-id="b09e2-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b09e2-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b09e2-165">Klient Printix tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="b09e2-165">Sign-on tooyour Printix tenant as an administrator.</span></span>

7. <span data-ttu-id="b09e2-166">V nabídce hello hello nahoře, klikněte na ikonu hello v pravém horním rohu hello a vyberte "**ověřování**".</span><span class="sxs-lookup"><span data-stu-id="b09e2-166">In hello menu on hello top, click hello icon at hello upper right corner and select "**Authentication**".</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. <span data-ttu-id="b09e2-168">Na hello **instalace** vyberte **ověřování povolit Azure nebo Office 365**</span><span class="sxs-lookup"><span data-stu-id="b09e2-168">On hello **Setup** tab, select **Enable Azure/Office 365 authentication**</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. <span data-ttu-id="b09e2-170">Na hello **Azure** kartě pole toohello adresa URL vstupu federační metadata z "**dokument federačních metadat**".</span><span class="sxs-lookup"><span data-stu-id="b09e2-170">On hello **Azure** tab, input federation metadata URL toohello textbox of "**Federation metadata document**".</span></span> 

    <span data-ttu-id="b09e2-171">Připojte soubor hello metadat xml, který jste stáhli z Azure AD příliš[tým podpory Printix](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="b09e2-171">Attach hello metadata xml file which you downloaded from Azure AD too[Printix support team](mailto:support@printix.net).</span></span> <span data-ttu-id="b09e2-172">Potom nahrát soubor xml hello a zadat adresu URL federačních metadat.</span><span class="sxs-lookup"><span data-stu-id="b09e2-172">Then they upload hello xml file and provide a federation metadata URL.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. <span data-ttu-id="b09e2-174">Klikněte na tlačítko hello "**testování**"tlačítko a klikněte na tlačítko"**OK**" tlačítko Pokud hello test proběhl úspěšně.</span><span class="sxs-lookup"><span data-stu-id="b09e2-174">Click hello "**Test**" button and click "**OK**" button if hello test was successful.</span></span>
   
     <span data-ttu-id="b09e2-175">Stránka služby Azure active directory se zobrazí po kliknutí na hello **testování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b09e2-175">Azure active directory page will show after clicking hello **test** button.</span></span> <span data-ttu-id="b09e2-176">"hello byl test úspěšný" zde znamená po zadání přihlašovacích údajů účtu Azure test, který bude pop hello se zobrazí zpráva "nastavení testovaný OK". Pak klikněte na tlačítko hello **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b09e2-176">"hello test was successful" here means after entering hello credentials of your Azure test account it will pop up a message "Settings tested OK".Then click hello **OK** button.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. <span data-ttu-id="b09e2-178">Klikněte na tlačítko hello **Uložit** na tlačítko "**ověřování**" stránky.</span><span class="sxs-lookup"><span data-stu-id="b09e2-178">Click hello **Save** button on "**Authentication**" page.</span></span>


> [!TIP]
> <span data-ttu-id="b09e2-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b09e2-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b09e2-180">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b09e2-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b09e2-181">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b09e2-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b09e2-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b09e2-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="b09e2-183">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b09e2-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b09e2-185">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b09e2-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b09e2-186">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b09e2-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b09e2-188">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b09e2-190">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b09e2-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b09e2-192">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b09e2-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b09e2-194">a.</span><span class="sxs-lookup"><span data-stu-id="b09e2-194">a.</span></span> <span data-ttu-id="b09e2-195">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b09e2-196">b.</span><span class="sxs-lookup"><span data-stu-id="b09e2-196">b.</span></span> <span data-ttu-id="b09e2-197">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b09e2-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b09e2-198">c.</span><span class="sxs-lookup"><span data-stu-id="b09e2-198">c.</span></span> <span data-ttu-id="b09e2-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b09e2-200">d.</span><span class="sxs-lookup"><span data-stu-id="b09e2-200">d.</span></span> <span data-ttu-id="b09e2-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-201">Click **Create**.</span></span>
 
### <a name="creating-a-printix-test-user"></a><span data-ttu-id="b09e2-202">Vytvoření zkušebního uživatele Printix</span><span class="sxs-lookup"><span data-stu-id="b09e2-202">Creating a Printix test user</span></span>

<span data-ttu-id="b09e2-203">Hello cílem této části je toocreate volal Britta Simon v Printix uživatele.</span><span class="sxs-lookup"><span data-stu-id="b09e2-203">hello objective of this section is toocreate a user called Britta Simon in Printix.</span></span> <span data-ttu-id="b09e2-204">Printix podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="b09e2-204">Printix supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b09e2-205">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="b09e2-205">There is no action item for you in this section.</span></span> <span data-ttu-id="b09e2-206">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Printix.</span><span class="sxs-lookup"><span data-stu-id="b09e2-206">A new user is created during an attempt tooaccess Printix if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="b09e2-207">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Printix](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="b09e2-207">If you need toocreate a user manually, you need toocontact hello [Printix support team](mailto:support@printix.net).</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b09e2-208">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b09e2-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b09e2-209">V této části povolíte tak, že udělíte přístup tooPrintix toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b09e2-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPrintix.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b09e2-211">**tooassign Britta Simon tooPrintix, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b09e2-211">**tooassign Britta Simon tooPrintix, perform hello following steps:**</span></span>

1. <span data-ttu-id="b09e2-212">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b09e2-214">V seznamu aplikace hello vyberte **Printix**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-214">In hello applications list, select **Printix**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. <span data-ttu-id="b09e2-216">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b09e2-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b09e2-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b09e2-218">Click **Add** button.</span></span> <span data-ttu-id="b09e2-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b09e2-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b09e2-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b09e2-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b09e2-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b09e2-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b09e2-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b09e2-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b09e2-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b09e2-224">Testing single sign-on</span></span>

<span data-ttu-id="b09e2-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b09e2-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b09e2-226">Když kliknete na dlaždici Printix hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Printix aplikace.</span><span class="sxs-lookup"><span data-stu-id="b09e2-226">When you click hello Printix tile in hello Access Panel, you should get automatically signed-on tooyour Printix application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b09e2-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b09e2-227">Additional resources</span></span>

* [<span data-ttu-id="b09e2-228">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b09e2-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b09e2-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b09e2-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

