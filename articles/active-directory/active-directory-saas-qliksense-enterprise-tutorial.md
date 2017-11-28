---
title: 'Kurz: Azure Active Directory integrace s Enterprise smysl Qlik | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Qlik smysl Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 153edb3f8cd41790d4e9a74f15cfb806e5e9e3b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a><span data-ttu-id="34975-103">Kurz: Azure Active Directory integrace s Qlik smysl Enterprise</span><span class="sxs-lookup"><span data-stu-id="34975-103">Tutorial: Azure Active Directory integration with Qlik Sense Enterprise</span></span>

<span data-ttu-id="34975-104">V tomto kurzu zjistíte, jak toointegrate Qlik smysl Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="34975-104">In this tutorial, you learn how toointegrate Qlik Sense Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="34975-105">Integrace Qlik smysl Enterprise s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="34975-105">Integrating Qlik Sense Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="34975-106">Můžete ovládat ve službě Azure AD, který má přístup tooQlik smysl Enterprise.</span><span class="sxs-lookup"><span data-stu-id="34975-106">You can control in Azure AD who has access tooQlik Sense Enterprise.</span></span>
- <span data-ttu-id="34975-107">Vaši uživatelé tooautomatically get přihlášeného tooQlik smysl Enterprise (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="34975-107">You can enable your users tooautomatically get signed-on tooQlik Sense Enterprise (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="34975-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="34975-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="34975-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="34975-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34975-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="34975-110">Prerequisites</span></span>

<span data-ttu-id="34975-111">tooconfigure integrace Azure AD s Enterprise Qlik smysl, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="34975-111">tooconfigure Azure AD integration with Qlik Sense Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="34975-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="34975-112">An Azure AD subscription</span></span>
- <span data-ttu-id="34975-113">Qlik smysl Enterprise jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="34975-113">A Qlik Sense Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="34975-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="34975-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="34975-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="34975-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="34975-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="34975-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="34975-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="34975-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="34975-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="34975-118">Scenario description</span></span>
<span data-ttu-id="34975-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="34975-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="34975-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="34975-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="34975-121">Přidání Qlik smysl Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="34975-121">Adding Qlik Sense Enterprise from hello gallery</span></span>
2. <span data-ttu-id="34975-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="34975-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-qlik-sense-enterprise-from-hello-gallery"></a><span data-ttu-id="34975-123">Přidání Qlik smysl Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="34975-123">Adding Qlik Sense Enterprise from hello gallery</span></span>
<span data-ttu-id="34975-124">tooconfigure hello integrace Qlik smysl Enterprise do Azure AD, je nutné tooadd Qlik smysl Enterprise hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="34975-124">tooconfigure hello integration of Qlik Sense Enterprise into Azure AD, you need tooadd Qlik Sense Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="34975-125">**tooadd Qlik smysl Enterprise z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="34975-125">**tooadd Qlik Sense Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="34975-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="34975-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="34975-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="34975-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="34975-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="34975-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="34975-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="34975-133">Hello vyhledávacího pole zadejte **Qlik smysl Enterprise**, vyberte **Qlik smysl Enterprise** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="34975-133">In hello search box, type **Qlik Sense Enterprise**, select **Qlik Sense Enterprise** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Enterprise Qlik smysl v seznamu výsledků hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="34975-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="34975-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="34975-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Enterprise smysl Qlik podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="34975-136">In this section, you configure and test Azure AD single sign-on with Qlik Sense Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="34975-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello protějšku uživatele v podniku smysl Qlik je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="34975-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Qlik Sense Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="34975-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Qlik smysl Enterprise musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="34975-138">In other words, a link relationship between an Azure AD user and hello related user in Qlik Sense Enterprise needs toobe established.</span></span>

<span data-ttu-id="34975-139">V Qlik smysl Enterprise, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="34975-139">In Qlik Sense Enterprise, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="34975-140">tooconfigure a testu Azure AD jednotné přihlašování s Enterprise Qlik smysl, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="34975-140">tooconfigure and test Azure AD single sign-on with Qlik Sense Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="34975-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="34975-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="34975-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="34975-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="34975-143">**[Vytvoření zkušebního uživatele Qlik smysl Enterprise](#create-a-qlik-sense-enterprise-test-user)**  -toohave protějšek Britta Simon v Qlik smysl organizace, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="34975-143">**[Create a Qlik Sense Enterprise test user](#create-a-qlik-sense-enterprise-test-user)** - toohave a counterpart of Britta Simon in Qlik Sense Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="34975-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="34975-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="34975-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="34975-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="34975-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="34975-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="34975-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Qlik smysl Enterprise.</span><span class="sxs-lookup"><span data-stu-id="34975-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Qlik Sense Enterprise application.</span></span>

<span data-ttu-id="34975-148">**tooconfigure Azure AD jednotné přihlašování s Qlik smysl Enterprise, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="34975-148">**tooconfigure Azure AD single sign-on with Qlik Sense Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="34975-149">V portálu Azure, na hello hello **Qlik smysl Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="34975-149">In hello Azure portal, on hello **Qlik Sense Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="34975-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="34975-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. <span data-ttu-id="34975-153">Na hello **Qlik smysl podnikové domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="34975-153">On hello **Qlik Sense Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![Qlik smysl podnikové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    <span data-ttu-id="34975-155">a.</span><span class="sxs-lookup"><span data-stu-id="34975-155">a.</span></span> <span data-ttu-id="34975-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span><span class="sxs-lookup"><span data-stu-id="34975-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="34975-157">Všimněte si hello koncové lomítko na konci hello tento identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="34975-157">Note hello trailing slash at hello end of this URI.</span></span> <span data-ttu-id="34975-158">Je vyžadována.</span><span class="sxs-lookup"><span data-stu-id="34975-158">It is required.</span></span>
    
    <span data-ttu-id="34975-159">b.</span><span class="sxs-lookup"><span data-stu-id="34975-159">b.</span></span> <span data-ttu-id="34975-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="34975-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > <span data-ttu-id="34975-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="34975-161">These values are not real.</span></span> <span data-ttu-id="34975-162">Aktualizace tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor, který je popsán později v tomto kurzu nebo kontaktujte [tým podpory klient systému Enterprise smysl Qlik](https://www.qlik.com/us/services/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="34975-162">Update these values with hello actual Sign-On URL and Identifier, Which are explained later in this tutorial or contact [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) tooget these values.</span></span> 

4. <span data-ttu-id="34975-163">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="34975-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. <span data-ttu-id="34975-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-165">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="34975-167">Připravte si soubor XML federační Metadata hello tak, aby tento server smysl tooQlik můžete nahrát.</span><span class="sxs-lookup"><span data-stu-id="34975-167">Prepare hello Federation Metadata XML file so that you can upload that tooQlik Sense server.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="34975-168">Před nahráním hello IdP metadata toohello Qlik smysl serveru, třeba soubor hello toobe upravit tooremove informace tooensure správné operace mezi službou Azure AD a server Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="34975-168">Before uploading hello IdP metadata toohello Qlik Sense server, hello file needs toobe edited tooremove information tooensure proper operation between Azure AD and Qlik Sense server.</span></span>
    
    ![QlikSense][qs24]
   
    <span data-ttu-id="34975-170">a.</span><span class="sxs-lookup"><span data-stu-id="34975-170">a.</span></span> <span data-ttu-id="34975-171">Otevřete soubor FederationMetaData.xml hello, který jste si stáhli z portálu Azure v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="34975-171">Open hello FederationMetaData.xml file, which you have downloaded from Azure portal in a text editor.</span></span>
   
    <span data-ttu-id="34975-172">b.</span><span class="sxs-lookup"><span data-stu-id="34975-172">b.</span></span> <span data-ttu-id="34975-173">Vyhledejte hodnotu hello **RoleDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="34975-173">Search for hello value **RoleDescriptor**.</span></span>  <span data-ttu-id="34975-174">Existují čtyři záznamy (dvě dvojice otevření a zavření značky elementu).</span><span class="sxs-lookup"><span data-stu-id="34975-174">There are four entries (two pairs of opening and closing element tags).</span></span>
   
    <span data-ttu-id="34975-175">c.</span><span class="sxs-lookup"><span data-stu-id="34975-175">c.</span></span> <span data-ttu-id="34975-176">Odstraníte hello RoleDescriptor značky a všechny informace v rozmezí ze souboru hello.</span><span class="sxs-lookup"><span data-stu-id="34975-176">Delete hello RoleDescriptor tags and all information in between from hello file.</span></span>
   
    <span data-ttu-id="34975-177">d.</span><span class="sxs-lookup"><span data-stu-id="34975-177">d.</span></span> <span data-ttu-id="34975-178">Uložte soubor hello a udržovat nedaleko pro použití později v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="34975-178">Save hello file and keep it nearby for use later in this document.</span></span>

7. <span data-ttu-id="34975-179">Jako uživatel, který můžete vytvořit virtuální proxy konfigurace přejděte toohello Qlik smysl Qlik Management Console (QMC).</span><span class="sxs-lookup"><span data-stu-id="34975-179">Navigate toohello Qlik Sense Qlik Management Console (QMC) as a user who can create virtual proxy configurations.</span></span>

8. <span data-ttu-id="34975-180">V hello QMC, klikněte na hello **virtuální proxy** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="34975-180">In hello QMC, click on hello **Virtual Proxies** menu item.</span></span>
   
    ![QlikSense][qs6] 

9. <span data-ttu-id="34975-182">V hello dolní části úvodní obrazovka, klikněte na položku hello **vytvořit nový** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-182">At hello bottom of hello screen, click hello **Create new** button.</span></span>
   
    ![QlikSense][qs7]

10. <span data-ttu-id="34975-184">Zobrazí se obrazovka úpravy proxy virtuálním Hello.</span><span class="sxs-lookup"><span data-stu-id="34975-184">hello Virtual proxy edit screen appears.</span></span>  <span data-ttu-id="34975-185">Na hello je pravé straně úvodní obrazovka nabídky pro viditelnosti možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="34975-185">On hello right side of hello screen is a menu for making configuration options visible.</span></span>
   
    ![QlikSense][qs9]

11. <span data-ttu-id="34975-187">Možnost nabídky identifikace hello zaškrtnuto zadejte hello identifikační informace pro konfiguraci Azure virtuální proxy hello.</span><span class="sxs-lookup"><span data-stu-id="34975-187">With hello Identification menu option checked, enter hello identifying information for hello Azure virtual proxy configuration.</span></span>
    
    ![QlikSense][qs8]  
    
    <span data-ttu-id="34975-189">a.</span><span class="sxs-lookup"><span data-stu-id="34975-189">a.</span></span> <span data-ttu-id="34975-190">Hello **popis** pole je popisný název pro konfiguraci proxy serveru virtuálním hello.</span><span class="sxs-lookup"><span data-stu-id="34975-190">hello **Description** field is a friendly name for hello virtual proxy configuration.</span></span>  <span data-ttu-id="34975-191">Zadejte hodnotu pro popis.</span><span class="sxs-lookup"><span data-stu-id="34975-191">Enter a value for a description.</span></span>
    
    <span data-ttu-id="34975-192">b.</span><span class="sxs-lookup"><span data-stu-id="34975-192">b.</span></span> <span data-ttu-id="34975-193">Hello **předpony** pole identifikuje koncový bod hello virtuální proxy pro připojení tooQlik smysl s Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="34975-193">hello **Prefix** field identifies hello virtual proxy endpoint for connecting tooQlik Sense with Azure AD Single Sign-On.</span></span>  <span data-ttu-id="34975-194">Zadejte název jedinečnou předponu pro tento virtuální server proxy.</span><span class="sxs-lookup"><span data-stu-id="34975-194">Enter a unique prefix name for this virtual proxy.</span></span>
    
    <span data-ttu-id="34975-195">c.</span><span class="sxs-lookup"><span data-stu-id="34975-195">c.</span></span> <span data-ttu-id="34975-196">**Časový limit nečinnosti relace (minuty)** hello vypršel časový limit pro připojení přes tuto virtuální proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="34975-196">**Session inactivity timeout (minutes)** is hello timeout for connections through this virtual proxy.</span></span>
    
    <span data-ttu-id="34975-197">d.</span><span class="sxs-lookup"><span data-stu-id="34975-197">d.</span></span> <span data-ttu-id="34975-198">Hello **název hlavičky souboru cookie relace** je ukládání název souboru cookie hello hello identifikátor relace pro hello Qlik smysl relace a uživatel obdrží po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="34975-198">hello **Session cookie header name** is hello cookie name storing hello session identifier for hello Qlik Sense session a user receives after successful authentication.</span></span>  <span data-ttu-id="34975-199">Tento název musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="34975-199">This name must be unique.</span></span>

12. <span data-ttu-id="34975-200">Klikněte na hello ověřování nabídky možnost toomake je viditelné.</span><span class="sxs-lookup"><span data-stu-id="34975-200">Click on hello Authentication menu option toomake it visible.</span></span>  <span data-ttu-id="34975-201">Zobrazí se obrazovka Hello ověřování.</span><span class="sxs-lookup"><span data-stu-id="34975-201">hello Authentication screen appears.</span></span>
    
    ![QlikSense][qs10]
    
    <span data-ttu-id="34975-203">a.</span><span class="sxs-lookup"><span data-stu-id="34975-203">a.</span></span> <span data-ttu-id="34975-204">Hello **anonymní přístup režimu** rozevíracího seznamu určuje Pokud anonymní uživatelé mohou přistupovat k Qlik smysl prostřednictvím proxy serveru virtuálním hello.</span><span class="sxs-lookup"><span data-stu-id="34975-204">hello **Anonymous access mode** drop down determines if anonymous users may access Qlik Sense through hello virtual proxy.</span></span>  <span data-ttu-id="34975-205">Hello výchozí možnost je žádné anonymního uživatele.</span><span class="sxs-lookup"><span data-stu-id="34975-205">hello default option is No anonymous user.</span></span>
    
    <span data-ttu-id="34975-206">b.</span><span class="sxs-lookup"><span data-stu-id="34975-206">b.</span></span> <span data-ttu-id="34975-207">Hello **metodu ověřování** rozevíracího seznamu určuje hello ověřování proxy schéma virtuální server hello použije.</span><span class="sxs-lookup"><span data-stu-id="34975-207">hello **Authentication method** drop-down determines hello authentication scheme hello virtual proxy will use.</span></span>  <span data-ttu-id="34975-208">Vyberte z rozevíracího seznamu hello SAML.</span><span class="sxs-lookup"><span data-stu-id="34975-208">Select SAML from hello drop-down list.</span></span>  <span data-ttu-id="34975-209">Proto se zobrazí další možnosti.</span><span class="sxs-lookup"><span data-stu-id="34975-209">More options appear as a result.</span></span>
    
    <span data-ttu-id="34975-210">c.</span><span class="sxs-lookup"><span data-stu-id="34975-210">c.</span></span> <span data-ttu-id="34975-211">V hello **pole URI hostitel SAML**, vstupní hello hostname uživatelé zadat tooaccess Qlik smysl prostřednictvím tento proxy server virtuální SAML.</span><span class="sxs-lookup"><span data-stu-id="34975-211">In hello **SAML host URI field**, input hello hostname users enter tooaccess Qlik Sense through this SAML virtual proxy.</span></span>  <span data-ttu-id="34975-212">název hostitele Hello je identifikátor uri hello hello Qlik smysl serveru.</span><span class="sxs-lookup"><span data-stu-id="34975-212">hello hostname is hello uri of hello Qlik Sense server.</span></span>
    
    <span data-ttu-id="34975-213">d.</span><span class="sxs-lookup"><span data-stu-id="34975-213">d.</span></span> <span data-ttu-id="34975-214">V hello **SAML entity ID**, zadejte hello stejnou hodnotu zadaná pro pole hello SAML hostitel identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="34975-214">In hello **SAML entity ID**, enter hello same value entered for hello SAML host URI field.</span></span>
    
    <span data-ttu-id="34975-215">e.</span><span class="sxs-lookup"><span data-stu-id="34975-215">e.</span></span> <span data-ttu-id="34975-216">Hello **SAML IdP metadata** je soubor hello upravit dříve v hello **upravit federačních metadat z konfigurace služby Azure AD** části.</span><span class="sxs-lookup"><span data-stu-id="34975-216">hello **SAML IdP metadata** is hello file edited earlier in hello **Edit Federation Metadata from Azure AD Configuration** section.</span></span>  <span data-ttu-id="34975-217">**Před nahráním hello IdP metadata, třeba soubor hello toobe upravit** tooremove informace tooensure správné operace mezi službou Azure AD a server Qlik smysl.</span><span class="sxs-lookup"><span data-stu-id="34975-217">**Before uploading hello IdP metadata, hello file needs toobe edited** tooremove information tooensure proper operation between Azure AD and Qlik Sense server.</span></span>  <span data-ttu-id="34975-218">**Pokud má soubor hello ještě upravit toobe najdete výše uvedené pokyny toohello.**</span><span class="sxs-lookup"><span data-stu-id="34975-218">**Please refer toohello instructions above if hello file has yet toobe edited.**</span></span>  <span data-ttu-id="34975-219">Pokud bylo upraveno hello souboru klikněte na hello tlačítko Procházet a vyberte hello upravená metadata souboru tooupload ho toohello konfigurace virtuálního serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="34975-219">If hello file has been edited click on hello Browse button and select hello edited metadata file tooupload it toohello virtual proxy configuration.</span></span>
    
    <span data-ttu-id="34975-220">f.</span><span class="sxs-lookup"><span data-stu-id="34975-220">f.</span></span> <span data-ttu-id="34975-221">Zadejte název nebo schématu odkaz atribut hello atributu SAML hello představující hello **UserID** toohello Qlik smysl server odešle Azure AD.</span><span class="sxs-lookup"><span data-stu-id="34975-221">Enter hello attribute name or schema reference for hello SAML attribute representing hello **UserID** Azure AD sends toohello Qlik Sense server.</span></span>  <span data-ttu-id="34975-222">Informace o schématu odkaz je k dispozici v konfiguraci post obrazovky aplikace Azure hello.</span><span class="sxs-lookup"><span data-stu-id="34975-222">Schema reference information is available in hello Azure app screens post configuration.</span></span>  <span data-ttu-id="34975-223">atribut name hello toouse, zadejte `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="34975-223">toouse hello name attribute, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="34975-224">g.</span><span class="sxs-lookup"><span data-stu-id="34975-224">g.</span></span> <span data-ttu-id="34975-225">Zadejte hodnotu hello hello **adresář uživatelského** , budou připojené toousers při ověření tooQlik smysl serveru prostřednictvím služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="34975-225">Enter hello value for hello **user directory** that will be attached toousers when they authenticate tooQlik Sense server through Azure AD.</span></span>  <span data-ttu-id="34975-226">Pevně zakódované hodnoty musí být uzavřena do **hranaté závorky []**.</span><span class="sxs-lookup"><span data-stu-id="34975-226">Hardcoded values must be surrounded by **square brackets []**.</span></span>  <span data-ttu-id="34975-227">toouse atribut odeslaných za hello kontrolního výrazu Azure AD SAML, zadejte do tohoto textového pole hello název atributu hello **bez** hranaté závorky.</span><span class="sxs-lookup"><span data-stu-id="34975-227">toouse an attribute sent in hello Azure AD SAML assertion, enter hello name of hello attribute in this text box **without** square brackets.</span></span>
    
    <span data-ttu-id="34975-228">h.</span><span class="sxs-lookup"><span data-stu-id="34975-228">h.</span></span> <span data-ttu-id="34975-229">Hello **SAML podpisový algoritmus** nastaví hello podepsání pro konfiguraci proxy serveru virtuálním hello certifikátu zprostředkovatele (v tomto případu Qlik smysl serveru).</span><span class="sxs-lookup"><span data-stu-id="34975-229">hello **SAML signing algorithm** sets hello service provider (in this case Qlik Sense server) certificate signing for hello virtual proxy configuration.</span></span>  <span data-ttu-id="34975-230">Pokud Qlik smysl serveru využívá důvěryhodné certifikát generovaný pomocí Microsoft Enhanced RSA a AES Cryptographic Provider, změňte hello SAML podpisový algoritmus příliš**SHA-256**.</span><span class="sxs-lookup"><span data-stu-id="34975-230">If Qlik Sense server uses a trusted certificate generated using Microsoft Enhanced RSA and AES Cryptographic Provider, change hello SAML signing algorithm too**SHA-256**.</span></span>
    
    <span data-ttu-id="34975-231">i.</span><span class="sxs-lookup"><span data-stu-id="34975-231">i.</span></span> <span data-ttu-id="34975-232">Hello části mapování atributů SAML umožňuje další atributy, jako jsou skupiny toobe odeslané tooQlik smysl pro použití v pravidla zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="34975-232">hello SAML attribute mapping section allows for additional attributes like groups toobe sent tooQlik Sense for use in security rules.</span></span>

13. <span data-ttu-id="34975-233">Klikněte na hello **Vyrovnávání zatížení** nabídky možnost toomake je viditelné.</span><span class="sxs-lookup"><span data-stu-id="34975-233">Click on hello **LOAD BALANCING** menu option toomake it visible.</span></span>  <span data-ttu-id="34975-234">Zobrazí se obrazovka Hello Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="34975-234">hello Load Balancing screen appears.</span></span>
    
    ![QlikSense][qs11]

14. <span data-ttu-id="34975-236">Klikněte na hello **přidat nový uzel serveru** tlačítko, vyberte modul uzlu nebo uzly Qlik smysl bude odeslat relací toofor zátěže účely a klikněte na hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-236">Click on hello **Add new server node** button, select engine node or nodes Qlik Sense will send sessions toofor load balancing purposes, and click hello **Add** button.</span></span>
    
    ![QlikSense][qs12]

15. <span data-ttu-id="34975-238">Klikněte na hello pokročilé nabídky možnost toomake je viditelné.</span><span class="sxs-lookup"><span data-stu-id="34975-238">Click on hello Advanced menu option toomake it visible.</span></span> <span data-ttu-id="34975-239">Zobrazí se obrazovka pokročilé Hello.</span><span class="sxs-lookup"><span data-stu-id="34975-239">hello Advanced screen appears.</span></span>
    
    ![QlikSense][qs13]
    
    <span data-ttu-id="34975-241">seznamu povolených Hello hostitele identifikuje názvy hostitelů, které jsou přijaty při připojování toohello Qlik smysl serveru.</span><span class="sxs-lookup"><span data-stu-id="34975-241">hello Host white list identifies hostnames that are accepted when connecting toohello Qlik Sense server.</span></span>  <span data-ttu-id="34975-242">**Zadejte název hostitele hello, které budou uživatelé zadávat při připojování serveru tooQlik smysl.**</span><span class="sxs-lookup"><span data-stu-id="34975-242">**Enter hello hostname users will specify when connecting tooQlik Sense server.**</span></span> <span data-ttu-id="34975-243">název hostitele Hello je hello stejnou hodnotu jako text hello uri SAML hostitele bez hello https://.</span><span class="sxs-lookup"><span data-stu-id="34975-243">hello hostname is hello same value as hello SAML host uri without hello https://.</span></span>

16. <span data-ttu-id="34975-244">Klikněte na tlačítko hello **použít** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-244">Click hello **Apply** button.</span></span>
    
    ![QlikSense][qs14]

17. <span data-ttu-id="34975-246">Klikněte na tlačítko OK tooaccept hello upozornění, které stavy proxy propojené toohello virtuální proxy se restartuje.</span><span class="sxs-lookup"><span data-stu-id="34975-246">Click OK tooaccept hello warning message that states proxies linked toohello virtual proxy will be restarted.</span></span>
    
    ![QlikSense][qs15]

18. <span data-ttu-id="34975-248">Na pravé straně hello úvodní obrazovka zobrazí se hello přidružené položky nabídky.</span><span class="sxs-lookup"><span data-stu-id="34975-248">On hello right side of hello screen, hello Associated items menu appears.</span></span>  <span data-ttu-id="34975-249">Klikněte na hello **proxy** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="34975-249">Click on hello **Proxies** menu option.</span></span>
    
    ![QlikSense][qs16]

19. <span data-ttu-id="34975-251">Zobrazí se obrazovka proxy Hello.</span><span class="sxs-lookup"><span data-stu-id="34975-251">hello proxy screen appears.</span></span>  <span data-ttu-id="34975-252">Klikněte na tlačítko hello **odkaz** tlačítko na hello dolní toolink toohello virtuální proxy server.</span><span class="sxs-lookup"><span data-stu-id="34975-252">Click hello **Link** button at hello bottom toolink a proxy toohello virtual proxy.</span></span>
    
    ![QlikSense][qs17]

20. <span data-ttu-id="34975-254">Vyberte hello proxy uzlu, který bude podporovat toto připojení virtuální proxy serveru a klikněte na tlačítko hello **odkaz** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-254">Select hello proxy node that will support this virtual proxy connection and click hello **Link** button.</span></span>  <span data-ttu-id="34975-255">Po propojení, se objeví pod přidružené proxy hello proxy.</span><span class="sxs-lookup"><span data-stu-id="34975-255">After linking, hello proxy will be listed under associated proxies.</span></span>
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. <span data-ttu-id="34975-258">Po přibližně pět sekund tooten, hello aktualizovat QMC zpráva se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="34975-258">After about five tooten seconds, hello Refresh QMC message will appear.</span></span>  <span data-ttu-id="34975-259">Klikněte na tlačítko hello **aktualizovat QMC** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-259">Click hello **Refresh QMC** button.</span></span>
    
    ![QlikSense][qs20]

22. <span data-ttu-id="34975-261">Když hello QMC aktualizuje, klikněte na hello **virtuální proxy** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="34975-261">When hello QMC refreshes, click on hello **Virtual proxies** menu item.</span></span> <span data-ttu-id="34975-262">Hello nové SAML virtuální proxy položku je uveden v tabulce hello na úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="34975-262">hello new SAML virtual proxy entry is listed in hello table on hello screen.</span></span>  <span data-ttu-id="34975-263">Jedním kliknutím na položku virtuální proxy hello.</span><span class="sxs-lookup"><span data-stu-id="34975-263">Single click on hello virtual proxy entry.</span></span>
    
    ![QlikSense][qs51]

23. <span data-ttu-id="34975-265">V hello dolní části obrazovky hello se budou aktivovat hello stáhnout SP metadata tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-265">At hello bottom of hello screen, hello Download SP metadata button will activate.</span></span>  <span data-ttu-id="34975-266">Klikněte na tlačítko hello **stáhnout SP metadata** soubor tooa tlačítko toosave hello metadat.</span><span class="sxs-lookup"><span data-stu-id="34975-266">Click hello **Download SP metadata** button toosave hello metadata tooa file.</span></span>
    
    ![QlikSense][qs52]

24. <span data-ttu-id="34975-268">Soubor metadat sp otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="34975-268">Open hello sp metadata file.</span></span>  <span data-ttu-id="34975-269">Sledovat hello **entityID** položku a hello **AssertionConsumerService** položku.</span><span class="sxs-lookup"><span data-stu-id="34975-269">Observe hello **entityID** entry and hello **AssertionConsumerService** entry.</span></span>  <span data-ttu-id="34975-270">Tyto hodnoty jsou ekvivalentní toohello **identifikátor** a hello **přihlásit na adrese URL** v konfiguraci aplikace hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="34975-270">These values are equivalent toohello **Identifier** and hello **Sign on URL** in hello Azure AD application configuration.</span></span> <span data-ttu-id="34975-271">Vložte tyto hodnoty hello **Qlik smysl podnikové domény a adresy URL** část v konfiguraci aplikace hello Azure AD, pokud jejich nejsou párování, pak by měl nahradíte je v Průvodci konfigurací hello aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="34975-271">Paste these values in hello **Qlik Sense Enterprise Domain and URLs** section in hello Azure AD application configuration if they are not matching, then you should replace them in hello Azure AD App configuration wizard.</span></span>
    
    ![QlikSense][qs53]

> [!TIP]
> <span data-ttu-id="34975-273">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="34975-273">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="34975-274">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="34975-274">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="34975-275">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="34975-275">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="34975-276">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="34975-276">Create an Azure AD test user</span></span>
<span data-ttu-id="34975-277">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="34975-277">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="34975-279">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="34975-279">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="34975-280">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-280">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

   ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="34975-282">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="34975-282">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

   ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="34975-284">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="34975-284">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

   ![tlačítko Přidat Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="34975-286">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="34975-286">In hello **User** dialog box, perform hello following steps:</span></span>

   ![Dialogové okno uživatelského Hello](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   <span data-ttu-id="34975-288">a.</span><span class="sxs-lookup"><span data-stu-id="34975-288">a.</span></span> <span data-ttu-id="34975-289">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="34975-289">In hello **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="34975-290">b.</span><span class="sxs-lookup"><span data-stu-id="34975-290">b.</span></span> <span data-ttu-id="34975-291">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="34975-291">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

   <span data-ttu-id="34975-292">c.</span><span class="sxs-lookup"><span data-stu-id="34975-292">c.</span></span> <span data-ttu-id="34975-293">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="34975-293">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

   <span data-ttu-id="34975-294">d.</span><span class="sxs-lookup"><span data-stu-id="34975-294">d.</span></span> <span data-ttu-id="34975-295">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="34975-295">Click **Create**.</span></span>
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a><span data-ttu-id="34975-296">Vytvoření zkušebního uživatele Qlik smysl Enterprise</span><span class="sxs-lookup"><span data-stu-id="34975-296">Create a Qlik Sense Enterprise test user</span></span>

<span data-ttu-id="34975-297">V této části vytvoříte uživatele volal Britta Simon v Qlik smysl Enterprise.</span><span class="sxs-lookup"><span data-stu-id="34975-297">In this section, you create a user called Britta Simon in Qlik Sense Enterprise.</span></span> <span data-ttu-id="34975-298">Práce s [tým podpory klient systému Enterprise smysl Qlik](https://www.qlik.com/us/services/support) pro přidání uživatelů hello hello Qlik smysl Enterprise platformy.</span><span class="sxs-lookup"><span data-stu-id="34975-298">Work with [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to add hello users in hello Qlik Sense Enterprise platform.</span></span> <span data-ttu-id="34975-299">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="34975-299">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="34975-300">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="34975-300">Assign hello Azure AD test user</span></span>

<span data-ttu-id="34975-301">V této části povolíte tak, že udělíte přístup tooQlik smysl Enterprise toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="34975-301">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooQlik Sense Enterprise.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="34975-303">**tooassign tooQlik Britta Simon smysl Enterprise, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="34975-303">**tooassign Britta Simon tooQlik Sense Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="34975-304">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="34975-304">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="34975-306">V seznamu aplikace hello vyberte **Qlik smysl Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="34975-306">In hello applications list, select **Qlik Sense Enterprise**.</span></span>

    ![Hello Qlik smysl Enterprise odkaz v seznamu aplikace hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. <span data-ttu-id="34975-308">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="34975-308">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="34975-310">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34975-310">Click **Add** button.</span></span> <span data-ttu-id="34975-311">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="34975-311">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="34975-313">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="34975-313">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="34975-314">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="34975-314">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="34975-315">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="34975-315">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="34975-316">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="34975-316">Test single sign-on</span></span>

<span data-ttu-id="34975-317">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="34975-317">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="34975-318">Když kliknete na dlaždici Qlik smysl Enterprise hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Qlik smysl podniková aplikace.</span><span class="sxs-lookup"><span data-stu-id="34975-318">When you click hello Qlik Sense Enterprise tile in hello Access Panel, you should get automatically signed-on tooyour Qlik Sense Enterprise application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="34975-319">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="34975-319">Additional resources</span></span>

* [<span data-ttu-id="34975-320">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="34975-320">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="34975-321">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="34975-321">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

