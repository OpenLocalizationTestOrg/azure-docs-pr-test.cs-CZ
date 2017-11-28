---
title: 'Kurz: Azure Active Directory integrace s T & E Express | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a T & E Express."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 869e5284c71904fcc817ceee0f39d94fab1bc6f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a><span data-ttu-id="eafa3-103">Kurz: Azure Active Directory integrace s T & E Express</span><span class="sxs-lookup"><span data-stu-id="eafa3-103">Tutorial: Azure Active Directory integration with T&E Express</span></span>

<span data-ttu-id="eafa3-104">V tomto kurzu zjistěte, jak integrovat T & E Express s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eafa3-104">In this tutorial, you learn how to integrate T&E Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eafa3-105">N & E Express integraci s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="eafa3-105">Integrating T&E Express with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="eafa3-106">Můžete řídit ve službě Azure AD, který má přístup k & E Express</span><span class="sxs-lookup"><span data-stu-id="eafa3-106">You can control in Azure AD who has access to T&E Express</span></span>
- <span data-ttu-id="eafa3-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k & E Express (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="eafa3-107">You can enable your users to automatically get signed-on to T&E Express (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="eafa3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="eafa3-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="eafa3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eafa3-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eafa3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eafa3-110">Prerequisites</span></span>

<span data-ttu-id="eafa3-111">Konfigurace integrace Azure AD s T & E Express, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="eafa3-111">To configure Azure AD integration with T&E Express, you need the following items:</span></span>

- <span data-ttu-id="eafa3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="eafa3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eafa3-113">T & E Express jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="eafa3-113">A T&E Express single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eafa3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="eafa3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eafa3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="eafa3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eafa3-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="eafa3-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="eafa3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eafa3-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eafa3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="eafa3-118">Scenario description</span></span>
<span data-ttu-id="eafa3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="eafa3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eafa3-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="eafa3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eafa3-121">Přidání T & E Express z Galerie</span><span class="sxs-lookup"><span data-stu-id="eafa3-121">Adding T&E Express from the gallery</span></span>
2. <span data-ttu-id="eafa3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="eafa3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-te-express-from-the-gallery"></a><span data-ttu-id="eafa3-123">Přidání T & E Express z Galerie</span><span class="sxs-lookup"><span data-stu-id="eafa3-123">Adding T&E Express from the gallery</span></span>
<span data-ttu-id="eafa3-124">Při konfiguraci integrace T & E Express do služby Azure AD, musíte přidat k & E Express z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="eafa3-124">To configure the integration of T&E Express into Azure AD, you need to add T&E Express from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eafa3-125">**Chcete-li přidat k & E Express z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eafa3-125">**To add T&E Express from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="eafa3-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="eafa3-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="eafa3-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="eafa3-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="eafa3-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eafa3-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="eafa3-133">Do vyhledávacího pole zadejte **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-133">In the search box, type **T&E Express**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. <span data-ttu-id="eafa3-135">Na panelu výsledků vyberte **T & E Express**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eafa3-135">In the results panel, select **T&E Express**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="eafa3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="eafa3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="eafa3-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s T & E Express podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="eafa3-138">In this section, you configure and test Azure AD single sign-on with T&E Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="eafa3-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v T & E Express je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eafa3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in T&E Express is to a user in Azure AD.</span></span> <span data-ttu-id="eafa3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v T & E Express je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="eafa3-140">In other words, a link relationship between an Azure AD user and the related user in T&E Express needs to be established.</span></span>

<span data-ttu-id="eafa3-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v T & E Express.</span><span class="sxs-lookup"><span data-stu-id="eafa3-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in T&E Express.</span></span>

<span data-ttu-id="eafa3-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s T & E Express, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="eafa3-142">To configure and test Azure AD single sign-on with T&E Express, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="eafa3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="eafa3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="eafa3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eafa3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eafa3-145">**[Vytvoření zkušebního uživatele T & E Express](#creating-a-te-express-test-user)**  – Pokud chcete mít protějšek Britta Simon v T & E Express, která je propojený s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="eafa3-145">**[Creating a T&E Express test user](#creating-a-te-express-test-user)** - to have a counterpart of Britta Simon in T&E Express that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="eafa3-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eafa3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eafa3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="eafa3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="eafa3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eafa3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="eafa3-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci T & E Express.</span><span class="sxs-lookup"><span data-stu-id="eafa3-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your T&E Express application.</span></span>

<span data-ttu-id="eafa3-150">**Ke konfiguraci Azure AD jednotné přihlašování s T & E Express, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eafa3-150">**To configure Azure AD single sign-on with T&E Express, perform the following steps:**</span></span>

1. <span data-ttu-id="eafa3-151">Na portálu Azure Management portal na **T & E Express** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-151">In the Azure Management portal, on the **T&E Express** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="eafa3-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="eafa3-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. <span data-ttu-id="eafa3-155">Na **T & E Express domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eafa3-155">On the **T&E Express Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    <span data-ttu-id="eafa3-157">a.</span><span class="sxs-lookup"><span data-stu-id="eafa3-157">a.</span></span> <span data-ttu-id="eafa3-158">V **identifikátor** textovému poli, zadejte hodnotu jako:`https://<domain>.tyeexpress.com`</span><span class="sxs-lookup"><span data-stu-id="eafa3-158">In the **Identifier** textbox, type the value as: `https://<domain>.tyeexpress.com`</span></span>

    <span data-ttu-id="eafa3-159">b.</span><span class="sxs-lookup"><span data-stu-id="eafa3-159">b.</span></span> <span data-ttu-id="eafa3-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span><span class="sxs-lookup"><span data-stu-id="eafa3-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="eafa3-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="eafa3-161">Please note that these are not the real values.</span></span> <span data-ttu-id="eafa3-162">Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="eafa3-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="eafa3-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="eafa3-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="eafa3-164">Obraťte se na [T & E Express tým podpory](http://www.tyeexpress.com/contacto.aspx) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="eafa3-164">Contact [T&E Express support team](http://www.tyeexpress.com/contacto.aspx) to get these values.</span></span>

5. <span data-ttu-id="eafa3-165">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="eafa3-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. <span data-ttu-id="eafa3-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eafa3-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="eafa3-169">Konfigurace jednotného přihlašování na **T & E Express** straně, přihlášení k T & E express aplikaci bez jednotného přihlašování SAML na pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="eafa3-169">To configure single sign-on on **T&E Express** side, login to the T&E express application without SAML single sign on using admin credentials.</span></span>

9. <span data-ttu-id="eafa3-170">V části **správce** klikněte na **SAML domény** otevřete stránku nastavení SAML.</span><span class="sxs-lookup"><span data-stu-id="eafa3-170">Under the **Admin** Tab, Click on **SAML domain** to Open the SAML settings page.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. <span data-ttu-id="eafa3-172">Vyberte **Activar(Activate)** možnost z **ne** k **SI(Yes)**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-172">Select the **Activar(Activate)** option from **No** to **SI(Yes)**.</span></span> <span data-ttu-id="eafa3-173">V **metadat zprostředkovatelů Identity** textovému poli, vložte metadata XML, které jste donwloaded z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eafa3-173">In the **Identity Provider Metadata** textbox, paste the metadata XML which you have donwloaded from Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. <span data-ttu-id="eafa3-175">Klikněte na **Guardar(Save)** tlačítko pro uložení nastavení.</span><span class="sxs-lookup"><span data-stu-id="eafa3-175">Click on the **Guardar(Save)** button to save the settings.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="eafa3-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="eafa3-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="eafa3-177">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eafa3-177">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="eafa3-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eafa3-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="eafa3-180">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="eafa3-180">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eafa3-182">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eafa3-182">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="eafa3-184">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eafa3-184">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eafa3-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eafa3-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eafa3-188">a.</span><span class="sxs-lookup"><span data-stu-id="eafa3-188">a.</span></span> <span data-ttu-id="eafa3-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eafa3-190">b.</span><span class="sxs-lookup"><span data-stu-id="eafa3-190">b.</span></span> <span data-ttu-id="eafa3-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="eafa3-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="eafa3-192">c.</span><span class="sxs-lookup"><span data-stu-id="eafa3-192">c.</span></span> <span data-ttu-id="eafa3-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="eafa3-194">d.</span><span class="sxs-lookup"><span data-stu-id="eafa3-194">d.</span></span> <span data-ttu-id="eafa3-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-195">Click **Create**.</span></span>
 
### <a name="creating-a-te-express-test-user"></a><span data-ttu-id="eafa3-196">Vytvoření zkušebního uživatele T & E Express</span><span class="sxs-lookup"><span data-stu-id="eafa3-196">Creating a T&E Express test user</span></span>

<span data-ttu-id="eafa3-197">Pokud chcete povolit uživatelům Azure AD přihlášení do n & E Express, musí být zřízená do n & E Express.</span><span class="sxs-lookup"><span data-stu-id="eafa3-197">In order to enable Azure AD users to log into T&E Express, they must be provisioned into T&E Express.</span></span>  
<span data-ttu-id="eafa3-198">V případě T & E Express zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="eafa3-198">In case of T&E Express, provisioning is a manual task.</span></span>

<span data-ttu-id="eafa3-199">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eafa3-199">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="eafa3-200">Přihlaste se k serveru vaší společnosti T & E Express jako správce.</span><span class="sxs-lookup"><span data-stu-id="eafa3-200">Log in to your T&E Express company site as an administrator.</span></span>

2. <span data-ttu-id="eafa3-201">V části značky správce klikněte na uživatelům otevřít uživatelé stránky předlohy.</span><span class="sxs-lookup"><span data-stu-id="eafa3-201">Under Admin tag, click on Users to open the Users master page.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. <span data-ttu-id="eafa3-203">Na domovské stránce klikněte na  **+**  přidejte uživatele.</span><span class="sxs-lookup"><span data-stu-id="eafa3-203">On the home page, click on **+** to add the users.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. <span data-ttu-id="eafa3-205">Zadejte povinné údaje, zobrazí výzva, ve formuláři a klikněte na tlačítko Uložit uložte podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="eafa3-205">Enter all the mandatory details as asked in the form and click the save button to save the details.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Můžete přidat zaměstnance](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="eafa3-208">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="eafa3-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="eafa3-209">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k & E Express.</span><span class="sxs-lookup"><span data-stu-id="eafa3-209">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to T&E Express.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="eafa3-211">**Britta Simon přiřadit k & E Express, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eafa3-211">**To assign Britta Simon to T&E Express, perform the following steps:**</span></span>

1. <span data-ttu-id="eafa3-212">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-212">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="eafa3-214">V seznamu aplikací vyberte **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-214">In the applications list, select **T&E Express**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. <span data-ttu-id="eafa3-216">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="eafa3-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="eafa3-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eafa3-218">Click **Add** button.</span></span> <span data-ttu-id="eafa3-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eafa3-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="eafa3-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eafa3-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="eafa3-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eafa3-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eafa3-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eafa3-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="eafa3-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eafa3-224">Testing single sign-on</span></span>

<span data-ttu-id="eafa3-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="eafa3-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="eafa3-226">Když kliknete na dlaždici T & E Express na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci T & E Express.</span><span class="sxs-lookup"><span data-stu-id="eafa3-226">When you click the T&E Express tile in the Access Panel, you should get automatically signed-on to your T&E Express application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eafa3-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="eafa3-227">Additional resources</span></span>

* [<span data-ttu-id="eafa3-228">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eafa3-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eafa3-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="eafa3-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png

