---
title: 'Kurz: Azure Active Directory integrace s Pingboard | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 008c670a8043da0c67ccefde48d5ef721c75d97c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a><span data-ttu-id="d7ec8-103">Kurz: Azure Active Directory integrace s Pingboard</span><span class="sxs-lookup"><span data-stu-id="d7ec8-103">Tutorial: Azure Active Directory integration with Pingboard</span></span>

<span data-ttu-id="d7ec8-104">V tomto kurzu zjistěte, jak integrovat Pingboard s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d7ec8-104">In this tutorial, you learn how to integrate Pingboard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d7ec8-105">Integrace Pingboard s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-105">Integrating Pingboard with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d7ec8-106">Můžete řídit ve službě Azure AD, který má přístup k Pingboard</span><span class="sxs-lookup"><span data-stu-id="d7ec8-106">You can control in Azure AD who has access to Pingboard</span></span>
- <span data-ttu-id="d7ec8-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Pingboard (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d7ec8-107">You can enable your users to automatically get signed-on to Pingboard (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d7ec8-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="d7ec8-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="d7ec8-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d7ec8-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7ec8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d7ec8-110">Prerequisites</span></span>

<span data-ttu-id="d7ec8-111">Konfigurace integrace Azure AD s Pingboard, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-111">To configure Azure AD integration with Pingboard, you need the following items:</span></span>

- <span data-ttu-id="d7ec8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d7ec8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d7ec8-113">Pingboard jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d7ec8-113">A Pingboard single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d7ec8-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d7ec8-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d7ec8-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="d7ec8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d7ec8-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d7ec8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d7ec8-118">Scenario description</span></span>
<span data-ttu-id="d7ec8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d7ec8-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d7ec8-121">Přidání Pingboard z Galerie</span><span class="sxs-lookup"><span data-stu-id="d7ec8-121">Adding Pingboard from the gallery</span></span>
2. <span data-ttu-id="d7ec8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d7ec8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pingboard-from-the-gallery"></a><span data-ttu-id="d7ec8-123">Přidání Pingboard z Galerie</span><span class="sxs-lookup"><span data-stu-id="d7ec8-123">Adding Pingboard from the gallery</span></span>
<span data-ttu-id="d7ec8-124">Při konfiguraci integrace Pingboard do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Pingboard z galerie.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-124">To configure the integration of Pingboard into Azure AD, you need to add Pingboard from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d7ec8-125">**Pokud chcete přidat Pingboard z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d7ec8-125">**To add Pingboard from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d7ec8-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d7ec8-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d7ec8-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d7ec8-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d7ec8-133">Do vyhledávacího pole zadejte **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-133">In the search box, type **Pingboard**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. <span data-ttu-id="d7ec8-135">Na panelu výsledků vyberte **Pingboard**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-135">In the results panel, select **Pingboard**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d7ec8-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d7ec8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d7ec8-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pingboard podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d7ec8-138">In this section, you configure and test Azure AD single sign-on with Pingboard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d7ec8-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Pingboard je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pingboard is to a user in Azure AD.</span></span> <span data-ttu-id="d7ec8-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Pingboard musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-140">In other words, a link relationship between an Azure AD user and the related user in Pingboard needs to be established.</span></span>

<span data-ttu-id="d7ec8-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Pingboard.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Pingboard.</span></span>

<span data-ttu-id="d7ec8-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Pingboard, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-142">To configure and test Azure AD single sign-on with Pingboard, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d7ec8-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d7ec8-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d7ec8-145">**[Vytvoření zkušebního uživatele Pingboard](#creating-a-pingboard-test-user)**  – Pokud chcete mít protějšek Britta Simon v Pingboard propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-145">**[Creating a Pingboard test user](#creating-a-pingboard-test-user)** - to have a counterpart of Britta Simon in Pingboard that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="d7ec8-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d7ec8-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d7ec8-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d7ec8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d7ec8-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Pingboard.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Pingboard application.</span></span>

<span data-ttu-id="d7ec8-150">**Ke konfiguraci Azure AD jednotné přihlašování s Pingboard, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d7ec8-150">**To configure Azure AD single sign-on with Pingboard, perform the following steps:**</span></span>

1. <span data-ttu-id="d7ec8-151">Na portálu Azure Management portal na **Pingboard** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-151">In the Azure Management portal, on the **Pingboard** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d7ec8-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. <span data-ttu-id="d7ec8-155">Na **Pingboard domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-155">On the **Pingboard Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    <span data-ttu-id="d7ec8-157">a.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-157">a.</span></span> <span data-ttu-id="d7ec8-158">V **identifikátor** textovému poli, zadejte hodnotu jako:`http://<entity-id>.pingboard.com/sp`</span><span class="sxs-lookup"><span data-stu-id="d7ec8-158">In the **Identifier** textbox, type the value as: `http://<entity-id>.pingboard.com/sp`</span></span>

    <span data-ttu-id="d7ec8-159">b.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-159">b.</span></span> <span data-ttu-id="d7ec8-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<entity-id>.pingboard.com/auth/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="d7ec8-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<entity-id>.pingboard.com/auth/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d7ec8-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-161">Please note that these are not the real values.</span></span> <span data-ttu-id="d7ec8-162">Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="d7ec8-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="d7ec8-164">Obraťte se na [tým podpory Pingboard klienta](https://support.pingboard.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-164">Contact [Pingboard Client support team](https://support.pingboard.com/) to get these values.</span></span> 

4. <span data-ttu-id="d7ec8-165">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-165">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    <span data-ttu-id="d7ec8-167">a.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-167">a.</span></span> <span data-ttu-id="d7ec8-168">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`http://<sub-domain>.pingboard.com/sign_in`</span><span class="sxs-lookup"><span data-stu-id="d7ec8-168">In the **Sign-on URL** textbox, type the value as: `http://<sub-domain>.pingboard.com/sign_in`</span></span>
     
5. <span data-ttu-id="d7ec8-169">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. <span data-ttu-id="d7ec8-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="d7ec8-173">Na straně Pingboard nakonfigurovat jednotné přihlašování, otevřete nové okno prohlížeče a přihlaste se k účtu Pingboard.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-173">To configure SSO on Pingboard side, open a new browser window and log in to your Pingboard Account.</span></span> <span data-ttu-id="d7ec8-174">Musí být správcem Pingboard nastavit jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-174">You must be a Pingboard admin to set up single sign on.</span></span>

8. <span data-ttu-id="d7ec8-175">V hlavní nabídce vyberte **aplikace > integrace**</span><span class="sxs-lookup"><span data-stu-id="d7ec8-175">From the top menu select **Apps > Integrations**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  <span data-ttu-id="d7ec8-177">Na **integrace** stránky, vyhledejte **"Azure Active Directory"** dlaždici a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-177">On the **Integrations** page, find the **"Azure Active Directory"** tile, and click it.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. <span data-ttu-id="d7ec8-179">V modální, který následuje dále klikněte na tlačítko **"Konfigurace"**</span><span class="sxs-lookup"><span data-stu-id="d7ec8-179">In the modal that follows click **"Configure"**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. <span data-ttu-id="d7ec8-181">Na následující stránce si všimnete, že "Integrace se službou Azure jednotného přihlašování k dispozici.".</span><span class="sxs-lookup"><span data-stu-id="d7ec8-181">On the following page, you will notice that "Azure SSO Integration is enabled.".</span></span> <span data-ttu-id="d7ec8-182">Otevřete soubor stažený soubor XML s metadaty v programu Poznámkový blok a vložte obsah **IDP Metadata**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-182">Open the downloaded Metadata XML file in a notepad and paste the content in **IDP Metadata**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. <span data-ttu-id="d7ec8-184">Soubor ověří a pokud se vše, co je správný, jednotného přihlašování na bude nyní povoleno</span><span class="sxs-lookup"><span data-stu-id="d7ec8-184">The file will be validated, and if everything is correct, single sign on will now be enabled</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d7ec8-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d7ec8-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="d7ec8-186">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-186">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d7ec8-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d7ec8-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d7ec8-189">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-189">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d7ec8-191">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-191">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d7ec8-193">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-193">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d7ec8-195">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d7ec8-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d7ec8-197">a.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-197">a.</span></span> <span data-ttu-id="d7ec8-198">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d7ec8-199">b.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-199">b.</span></span> <span data-ttu-id="d7ec8-200">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d7ec8-201">c.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-201">c.</span></span> <span data-ttu-id="d7ec8-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d7ec8-203">d.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-203">d.</span></span> <span data-ttu-id="d7ec8-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-204">Click **Create**.</span></span>
 
### <a name="creating-a-pingboard-test-user"></a><span data-ttu-id="d7ec8-205">Vytvoření zkušebního uživatele Pingboard</span><span class="sxs-lookup"><span data-stu-id="d7ec8-205">Creating a Pingboard test user</span></span>

<span data-ttu-id="d7ec8-206">Pokud chcete povolit uživatelům Azure AD přihlášení do Pingboard, musí být zřízená do Pingboard.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-206">In order to enable Azure AD users to log into Pingboard, they must be provisioned into Pingboard.</span></span>  
<span data-ttu-id="d7ec8-207">V případě Pingboard zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-207">In the case of Pingboard, provisioning is a manual task.</span></span>

<span data-ttu-id="d7ec8-208">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d7ec8-208">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="d7ec8-209">Přihlaste se k serveru vaší společnosti Pingboard jako správce.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-209">Log in to your Pingboard company site as an administrator.</span></span>

2. <span data-ttu-id="d7ec8-210">Klikněte na tlačítko **"Přidat zaměstnancem"** tlačítko **Directory** stránky.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-210">Click **“Add Employee”** button on **Directory** page.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. <span data-ttu-id="d7ec8-212">Na **"Přidat zaměstnancem"** dialogové okno proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-212">On the **“Add Employee”** dialog page, perform the following steps.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    <span data-ttu-id="d7ec8-214">a.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-214">a.</span></span> <span data-ttu-id="d7ec8-215">V **úplný název** textovému poli, zadejte úplný název Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-215">In the **Full Name** textbox, type the full name of Britta Simon.</span></span>

    <span data-ttu-id="d7ec8-216">b.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-216">b.</span></span> <span data-ttu-id="d7ec8-217">V **e-mailu** textovému poli, zadejte e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-217">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="d7ec8-218">c.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-218">c.</span></span> <span data-ttu-id="d7ec8-219">V **funkce** textovému poli, zadejte název úlohy Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-219">In the **Job Title** textbox, type the job title of Britta Simon.</span></span>

    <span data-ttu-id="d7ec8-220">d.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-220">d.</span></span> <span data-ttu-id="d7ec8-221">V **umístění** rozevíracího seznamu, vyberte umístění Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-221">In the **Location** dropdown, select the location  of Britta Simon.</span></span>
    
    <span data-ttu-id="d7ec8-222">e.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-222">e.</span></span> <span data-ttu-id="d7ec8-223">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-223">Click **Add**.</span></span>   

4. <span data-ttu-id="d7ec8-224">Obrazovka s potvrzením bude spuštěna potvrďte přidání uživatele.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-224">A confirmation screen will come up to confirm the addition of user.</span></span>
    
    ![Potvrďte](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > <span data-ttu-id="d7ec8-226">Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-226">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d7ec8-227">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d7ec8-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d7ec8-228">V této části povolíte Britta Simon používat tak, že udělíte přístup k Pingboard Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-228">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Pingboard.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d7ec8-230">**Pokud chcete přiřadit Britta Simon Pingboard, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d7ec8-230">**To assign Britta Simon to Pingboard, perform the following steps:**</span></span>

1. <span data-ttu-id="d7ec8-231">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-231">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d7ec8-233">V seznamu aplikací vyberte **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-233">In the applications list, select **Pingboard**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. <span data-ttu-id="d7ec8-235">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d7ec8-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-237">Click **Add** button.</span></span> <span data-ttu-id="d7ec8-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d7ec8-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d7ec8-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d7ec8-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d7ec8-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d7ec8-243">Testing single sign-on</span></span>

<span data-ttu-id="d7ec8-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d7ec8-245">Když kliknete na dlaždici Pingboard na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Pingboard.</span><span class="sxs-lookup"><span data-stu-id="d7ec8-245">When you click the Pingboard tile in the Access Panel, you should get automatically signed-on to your Pingboard application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d7ec8-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d7ec8-246">Additional resources</span></span>

* [<span data-ttu-id="d7ec8-247">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d7ec8-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d7ec8-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d7ec8-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
