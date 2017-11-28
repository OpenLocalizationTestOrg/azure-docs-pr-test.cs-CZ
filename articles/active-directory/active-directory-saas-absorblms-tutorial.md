---
title: "Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a vyrovná se se zatížením pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: a140a78a3a9474a6372a3ad4fb8251bd2452c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a><span data-ttu-id="8d938-103">Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="8d938-103">Tutorial: Azure Active Directory integration with Absorb LMS</span></span>

<span data-ttu-id="8d938-104">V tomto kurzu zjistíte, jak toointegrate Absorb pro správu vzdělávacího procesu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8d938-104">In this tutorial, you learn how toointegrate Absorb LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8d938-105">Integrace vyrovná se se zatížením pro správu vzdělávacího procesu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8d938-105">Integrating Absorb LMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8d938-106">Můžete řídit ve službě Azure AD, který má tooAbsorb přístup pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="8d938-106">You can control in Azure AD who has access tooAbsorb LMS</span></span>
- <span data-ttu-id="8d938-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAbsorb pro správu vzdělávacího procesu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d938-107">You can enable your users tooautomatically get signed-on tooAbsorb LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8d938-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8d938-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8d938-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="8d938-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="8d938-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8d938-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d938-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8d938-111">Prerequisites</span></span>

<span data-ttu-id="8d938-112">tooconfigure integrace Azure AD s vyrovná se se zatížením pro správu vzdělávacího procesu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8d938-112">tooconfigure Azure AD integration with Absorb LMS, you need hello following items:</span></span>

- <span data-ttu-id="8d938-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d938-113">An Azure AD subscription</span></span>
- <span data-ttu-id="8d938-114">Vyrovná se se zatížením pro správu vzdělávacího procesu jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8d938-114">An Absorb LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8d938-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8d938-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8d938-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8d938-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8d938-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8d938-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8d938-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8d938-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8d938-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8d938-119">Scenario description</span></span>
<span data-ttu-id="8d938-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8d938-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8d938-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8d938-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8d938-122">Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8d938-122">Adding Absorb LMS from hello gallery</span></span>
2. <span data-ttu-id="8d938-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d938-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-absorb-lms-from-hello-gallery"></a><span data-ttu-id="8d938-124">Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8d938-124">Adding Absorb LMS from hello gallery</span></span>
<span data-ttu-id="8d938-125">tooconfigure hello integrace vyrovná se se zatížením pro správu vzdělávacího procesu v tooAzure AD, je nutné tooadd Absorb pro správu vzdělávacího procesu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8d938-125">tooconfigure hello integration of Absorb LMS in tooAzure AD, you need tooadd Absorb LMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8d938-126">**tooadd Absorb pro správu vzdělávacího procesu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d938-126">**tooadd Absorb LMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d938-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8d938-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="8d938-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8d938-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8d938-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8d938-130">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="8d938-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d938-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="8d938-134">Hello vyhledávacího pole zadejte **vyrovná se se zatížením pro správu vzdělávacího procesu**, vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d938-134">In hello search box, type **Absorb LMS**, select **Absorb LMS** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Vyrovná se se zatížením pro správu vzdělávacího procesu v seznamu výsledků hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8d938-136">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d938-136">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="8d938-137">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s vyrovná se se zatížením vzdělávacího procesu na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8d938-137">In this section, you configure and test Azure AD single sign-on with Absorb LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8d938-138">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v vyrovná se se zatížením pro správu vzdělávacího procesu je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8d938-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Absorb LMS is tooa user in Azure AD.</span></span> <span data-ttu-id="8d938-139">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v vyrovná se se zatížením pro správu vzdělávacího procesu musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8d938-139">In other words, a link relationship between an Azure AD user and hello related user in Absorb LMS needs toobe established.</span></span>

<span data-ttu-id="8d938-140">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="8d938-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Absorb LMS.</span></span>

<span data-ttu-id="8d938-141">tooconfigure a testu Azure AD jednotné přihlašování s vyrovná se se zatížením pro správu vzdělávacího procesu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="8d938-141">tooconfigure and test Azure AD single sign-on with Absorb LMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8d938-142">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8d938-142">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8d938-143">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8d938-143">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8d938-144">**[Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu](#create-an-absorb-lms-test-user)**  -toohave protějšek Britta Simon v vzdělávacího procesu vyrovná se se zatížením, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8d938-144">**[Create an Absorb LMS test user](#create-an-absorb-lms-test-user)** - toohave a counterpart of Britta Simon in Absorb LMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8d938-145">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8d938-145">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8d938-146">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8d938-146">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8d938-147">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d938-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8d938-148">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="8d938-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Absorb LMS application.</span></span>

<span data-ttu-id="8d938-149">**tooconfigure Azure AD jednotné přihlašování s vzdělávacího procesu vyrovná se se zatížením, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d938-149">**tooconfigure Azure AD single sign-on with Absorb LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d938-150">V portálu Azure, na hello hello **vyrovná se se zatížením pro správu vzdělávacího procesu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8d938-150">In hello Azure portal, on hello **Absorb LMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="8d938-152">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8d938-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. <span data-ttu-id="8d938-154">Na hello **vyrovná se se zatížením pro správu vzdělávacího procesu domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8d938-154">On hello **Absorb LMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Pro správu vzdělávacího procesu adresy URL jeden přihlašování informace o doméně a vyrovná se se zatížením](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    <span data-ttu-id="8d938-156">a.</span><span class="sxs-lookup"><span data-stu-id="8d938-156">a.</span></span> <span data-ttu-id="8d938-157">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="8d938-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>

    <span data-ttu-id="8d938-158">b.</span><span class="sxs-lookup"><span data-stu-id="8d938-158">b.</span></span> <span data-ttu-id="8d938-159">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="8d938-159">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="8d938-160">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="8d938-160">These values are not hello real.</span></span> <span data-ttu-id="8d938-161">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8d938-161">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="8d938-162">Obraťte se na [tým podpory vyrovná se se zatížením klienta pro správu vzdělávacího procesu](https://www.absorblms.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8d938-162">Contact [Absorb LMS Client support team](https://www.absorblms.com/support) tooget these values.</span></span> 

4. <span data-ttu-id="8d938-163">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8d938-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. <span data-ttu-id="8d938-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d938-165">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="8d938-167">Na hello **vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu** klikněte na tlačítko **konfigurace vyrovná se se zatížením vzdělávacího procesu** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8d938-167">On hello **Absorb LMS Configuration** section, click **Configure Absorb LMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8d938-168">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8d938-168">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. <span data-ttu-id="8d938-170">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="8d938-170">In a different web browser window, log in tooyour Absorb LMS company site as an administrator.</span></span>

9. <span data-ttu-id="8d938-171">Klikněte na tlačítko hello **ikonu účtu** na rozhraní správce hello.</span><span class="sxs-lookup"><span data-stu-id="8d938-171">Click hello **Account Icon** on hello admin interface.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/1.png)

10. <span data-ttu-id="8d938-173">Klikněte na tlačítko **nastavení portálu**.</span><span class="sxs-lookup"><span data-stu-id="8d938-173">Click **Portal Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. <span data-ttu-id="8d938-175">Klikněte na tlačítko hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="8d938-175">Click hello **Users** tab.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/3.png)

12. <span data-ttu-id="8d938-177">Proveďte následující kroky tooaccess hello jednotné přihlašování konfigurační pole hello:</span><span class="sxs-lookup"><span data-stu-id="8d938-177">Perform hello following steps tooaccess hello Single Sign-On configuration fields:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/4.png)

    <span data-ttu-id="8d938-179">a.</span><span class="sxs-lookup"><span data-stu-id="8d938-179">a.</span></span> <span data-ttu-id="8d938-180">Vyberte odpovídající hello **režimu**.</span><span class="sxs-lookup"><span data-stu-id="8d938-180">Select hello appropriate **Mode**.</span></span>

    <span data-ttu-id="8d938-181">b.</span><span class="sxs-lookup"><span data-stu-id="8d938-181">b.</span></span> <span data-ttu-id="8d938-182">Otevřete hello certifikát, který jste si stáhli z portálu Azure v poznámkovém bloku hello odebrat hello **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---** značku a potom vložte hello zbývající obsah Hello **klíč** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8d938-182">Open hello Certificate that you have downloaded from hello Azure portal in notepad, remove hello **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste hello remaining content in hello **Key** textbox.</span></span>
    
    <span data-ttu-id="8d938-183">c.</span><span class="sxs-lookup"><span data-stu-id="8d938-183">c.</span></span> <span data-ttu-id="8d938-184">V hello **Vlastnost Id**, vyberte hello odpovídající atribut, který jste nakonfigurovali jako hello identifikátor uživatele v hello Azure AD (například pokud hello userprinciplename je vybrána ve službě Azure AD, pak uživatelské jméno by zde vybraný.)</span><span class="sxs-lookup"><span data-stu-id="8d938-184">In hello **Id Property**, select hello appropriate attribute which you have configured as hello user identifier in hello Azure AD (For example, If hello userprinciplename is selected in Azure AD, then Username would be selected here.)</span></span>

    <span data-ttu-id="8d938-185">d.</span><span class="sxs-lookup"><span data-stu-id="8d938-185">d.</span></span> <span data-ttu-id="8d938-186">V hello **přihlašovací adresa URL**, vložte hello **"SAML jeden přihlašování adresa URL služby"** hodnotu zkopírovanou z hello **konfigurovat přihlášení** okno hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8d938-186">In hello **Login URL**, paste hello **“SAML Single Sign-On Service URL”** value you have copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

    <span data-ttu-id="8d938-187">e.</span><span class="sxs-lookup"><span data-stu-id="8d938-187">e.</span></span> <span data-ttu-id="8d938-188">V hello **adresy URL odhlašovací**, vložte hello **"Adresa URL Sign-Out"** hodnotu zkopírovanou z hello **konfigurovat přihlášení** okno hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8d938-188">In hello **Logout URL**, paste hello **“Sign-Out URL”** value you have copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

13. <span data-ttu-id="8d938-189">Povolit **"Povolit jenom přihlášení SSO"**.</span><span class="sxs-lookup"><span data-stu-id="8d938-189">Enable **‘Only Allow SSO Login’**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/5.png)

14. <span data-ttu-id="8d938-191">Klikněte na tlačítko **"Uložit."**</span><span class="sxs-lookup"><span data-stu-id="8d938-191">Click **"Save."**</span></span>

> [!TIP]
> <span data-ttu-id="8d938-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8d938-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8d938-193">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8d938-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8d938-194">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8d938-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8d938-195">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d938-195">Create an Azure AD test user</span></span>

<span data-ttu-id="8d938-196">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8d938-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="8d938-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d938-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d938-199">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8d938-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8d938-201">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8d938-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8d938-203">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8d938-203">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8d938-205">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8d938-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8d938-207">a.</span><span class="sxs-lookup"><span data-stu-id="8d938-207">a.</span></span> <span data-ttu-id="8d938-208">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8d938-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8d938-209">b.</span><span class="sxs-lookup"><span data-stu-id="8d938-209">b.</span></span> <span data-ttu-id="8d938-210">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8d938-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8d938-211">c.</span><span class="sxs-lookup"><span data-stu-id="8d938-211">c.</span></span> <span data-ttu-id="8d938-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8d938-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8d938-213">d.</span><span class="sxs-lookup"><span data-stu-id="8d938-213">d.</span></span> <span data-ttu-id="8d938-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8d938-214">Click **Create**.</span></span>

### <a name="create-an-absorb-lms-test-user"></a><span data-ttu-id="8d938-215">Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="8d938-215">Create an Absorb LMS test user</span></span>

<span data-ttu-id="8d938-216">Uživatelé toolog tooenable Azure AD v tooAbsorb pro správu vzdělávacího procesu, se musí být zřízená v tooAbsorb pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="8d938-216">tooenable Azure AD users toolog in tooAbsorb LMS, they must be provisioned in tooAbsorb LMS.</span></span>  
<span data-ttu-id="8d938-217">Pro vyrovná se se zatížením vzdělávacího procesu zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="8d938-217">For Absorb LMS, provisioning is a manual task.</span></span>

<span data-ttu-id="8d938-218">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d938-218">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d938-219">Přihlaste se tooyour vyrovná se se zatížením pro správu vzdělávacího procesu společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="8d938-219">Log in tooyour Absorb LMS company site as an administrator.</span></span>

2. <span data-ttu-id="8d938-220">Klikněte na tlačítko **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="8d938-220">Click **Users** tab.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. <span data-ttu-id="8d938-222">Klikněte na tlačítko **uživatelé** pod hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="8d938-222">Click **Users** under hello **Users** tab.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  <span data-ttu-id="8d938-224">Vyberte **uživatele** z **přidat nové** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="8d938-224">Select **User** from **Add New** drop-down.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. <span data-ttu-id="8d938-226">Na hello **přidat uživatele** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8d938-226">On hello **Add User** page, perform hello following steps:</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/user.png)

    <span data-ttu-id="8d938-228">a.</span><span class="sxs-lookup"><span data-stu-id="8d938-228">a.</span></span> <span data-ttu-id="8d938-229">V hello **křestní jméno** textovému poli, typ hello křestní jméno jako Britta.</span><span class="sxs-lookup"><span data-stu-id="8d938-229">In hello **First Name** textbox, type hello first name like Britta.</span></span>

    <span data-ttu-id="8d938-230">b.</span><span class="sxs-lookup"><span data-stu-id="8d938-230">b.</span></span> <span data-ttu-id="8d938-231">V hello **příjmení** textovému poli, typ hello příjmení jako Simon.</span><span class="sxs-lookup"><span data-stu-id="8d938-231">In hello **Last Name** textbox, type hello last name like Simon.</span></span>
    
    <span data-ttu-id="8d938-232">c.</span><span class="sxs-lookup"><span data-stu-id="8d938-232">c.</span></span> <span data-ttu-id="8d938-233">V hello **uživatelské jméno** textovému poli, zadejte jméno uživatele hello jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8d938-233">In hello **Username** textbox, type hello user name like Britta Simon.</span></span>

    <span data-ttu-id="8d938-234">d.</span><span class="sxs-lookup"><span data-stu-id="8d938-234">d.</span></span> <span data-ttu-id="8d938-235">V hello **heslo** textovému poli, zadejte heslo hello Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8d938-235">In hello **Password** textbox, type hello password of Britta Simon.</span></span>

    <span data-ttu-id="8d938-236">e.</span><span class="sxs-lookup"><span data-stu-id="8d938-236">e.</span></span> <span data-ttu-id="8d938-237">V hello **Potvrdit heslo** textovému poli, typ hello stejné heslo.</span><span class="sxs-lookup"><span data-stu-id="8d938-237">In hello **Confirm Password** textbox, type hello same password.</span></span>
    
    <span data-ttu-id="8d938-238">f.</span><span class="sxs-lookup"><span data-stu-id="8d938-238">f.</span></span> <span data-ttu-id="8d938-239">Nastavit jej jako **ACTIVE**.</span><span class="sxs-lookup"><span data-stu-id="8d938-239">Make it as **ACTIVE**.</span></span>   

6. <span data-ttu-id="8d938-240">Klikněte na tlačítko **"Uložit."**</span><span class="sxs-lookup"><span data-stu-id="8d938-240">Click **"Save."**</span></span>
 
### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="8d938-241">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8d938-241">Assign hello Azure AD test user</span></span>

<span data-ttu-id="8d938-242">V této části povolíte tak, že udělíte přístup tooAbsorb pro správu vzdělávacího procesu Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8d938-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAbsorb LMS.</span></span>

![Přiřadit role uživatele hello][200]

<span data-ttu-id="8d938-244">**tooassign tooAbsorb Britta Simon vzdělávacího procesu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d938-244">**tooassign Britta Simon tooAbsorb LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d938-245">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8d938-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8d938-247">V seznamu aplikace hello vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu**.</span><span class="sxs-lookup"><span data-stu-id="8d938-247">In hello applications list, select **Absorb LMS**.</span></span>

    ![Hello vyrovná se se zatížením pro správu vzdělávacího procesu odkaz v seznamu aplikace hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. <span data-ttu-id="8d938-249">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8d938-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="8d938-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d938-251">Click **Add** button.</span></span> <span data-ttu-id="8d938-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8d938-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="8d938-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8d938-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8d938-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8d938-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8d938-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8d938-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8d938-257">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d938-257">Test single sign-on</span></span>

<span data-ttu-id="8d938-258">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8d938-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8d938-259">Klikněte na tlačítko hello vyrovná se se zatížením pro správu vzdělávacího procesu dlaždici v hello přístupového panelu, zobrazí se automaticky přihlášeného tooyour vyrovná se se zatížením pro správu vzdělávacího procesu aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d938-259">Click hello Absorb LMS tile in hello Access Panel, you will get automatically signed-on tooyour Absorb LMS application.</span></span> <span data-ttu-id="8d938-260">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="8d938-260">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d938-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8d938-261">Additional resources</span></span>

* [<span data-ttu-id="8d938-262">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8d938-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8d938-263">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8d938-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

