---
title: 'Kurz: Azure Active Directory integrace s lidmi | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a osoby."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c9b6202-11dd-4bb6-a679-8fb0a7a0ef4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: caa287a2ed8774965ef722685e4e950336e5e0ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-people"></a><span data-ttu-id="756e4-103">Kurz: Azure Active Directory integrace s uživateli</span><span class="sxs-lookup"><span data-stu-id="756e4-103">Tutorial: Azure Active Directory integration with People</span></span>

<span data-ttu-id="756e4-104">V tomto kurzu zjistěte, jak integrovat osoby s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="756e4-104">In this tutorial, you learn how to integrate People with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="756e4-105">Integrace osoby s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="756e4-105">Integrating People with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="756e4-106">Můžete řídit ve službě Azure AD, který má přístup osobám</span><span class="sxs-lookup"><span data-stu-id="756e4-106">You can control in Azure AD who has access to People</span></span>
- <span data-ttu-id="756e4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k osoby (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="756e4-107">You can enable your users to automatically get signed-on to People (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="756e4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="756e4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="756e4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="756e4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="756e4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="756e4-110">Prerequisites</span></span>

<span data-ttu-id="756e4-111">Ke konfiguraci integrace služby Azure AD s lidmi, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="756e4-111">To configure Azure AD integration with People, you need the following items:</span></span>

- <span data-ttu-id="756e4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="756e4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="756e4-113">Osoby jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="756e4-113">A People single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="756e4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="756e4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="756e4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="756e4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="756e4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="756e4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="756e4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="756e4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="756e4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="756e4-118">Scenario description</span></span>
<span data-ttu-id="756e4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="756e4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="756e4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="756e4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="756e4-121">Přidání uživatelů z Galerie</span><span class="sxs-lookup"><span data-stu-id="756e4-121">Adding People from the gallery</span></span>
2. <span data-ttu-id="756e4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="756e4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-people-from-the-gallery"></a><span data-ttu-id="756e4-123">Přidání uživatelů z Galerie</span><span class="sxs-lookup"><span data-stu-id="756e4-123">Adding People from the gallery</span></span>
<span data-ttu-id="756e4-124">Při konfiguraci integrace uživatelů do služby Azure AD, musíte přidat osoby z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="756e4-124">To configure the integration of People into Azure AD, you need to add People from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="756e4-125">**Pokud chcete přidat osoby z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="756e4-125">**To add People from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="756e4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="756e4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="756e4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="756e4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="756e4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="756e4-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="756e4-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="756e4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="756e4-133">Do vyhledávacího pole zadejte **osoby**.</span><span class="sxs-lookup"><span data-stu-id="756e4-133">In the search box, type **People**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_search.png)

5. <span data-ttu-id="756e4-135">Na panelu výsledků vyberte **osoby**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="756e4-135">In the results panel, select **People**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="756e4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="756e4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="756e4-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s uživateli na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="756e4-138">In this section, you configure and test Azure AD single sign-on with People based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="756e4-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v osoby je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="756e4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in People is to a user in Azure AD.</span></span> <span data-ttu-id="756e4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v osoby musí navázat.</span><span class="sxs-lookup"><span data-stu-id="756e4-140">In other words, a link relationship between an Azure AD user and the related user in People needs to be established.</span></span>

<span data-ttu-id="756e4-141">V osoby, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="756e4-141">In People, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="756e4-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s lidmi, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="756e4-142">To configure and test Azure AD single sign-on with People, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="756e4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="756e4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="756e4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="756e4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="756e4-145">**[Vytvoření zkušebního uživatele osoby](#creating-a-people-test-user)**  – Pokud chcete mít protějšek Britta Simon v osoby, které je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="756e4-145">**[Creating a People test user](#creating-a-people-test-user)** - to have a counterpart of Britta Simon in People that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="756e4-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="756e4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="756e4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="756e4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="756e4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="756e4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="756e4-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci osoby.</span><span class="sxs-lookup"><span data-stu-id="756e4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your People application.</span></span>

<span data-ttu-id="756e4-150">**Ke konfiguraci Azure AD jednotné přihlašování s lidmi, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="756e4-150">**To configure Azure AD single sign-on with People, perform the following steps:**</span></span>

1. <span data-ttu-id="756e4-151">Na portálu Azure na **osoby** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="756e4-151">In the Azure portal, on the **People** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="756e4-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="756e4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_samlbase.png)

3. <span data-ttu-id="756e4-155">Na **uživatelé domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="756e4-155">On the **People Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_url.png)

    <span data-ttu-id="756e4-157">a.</span><span class="sxs-lookup"><span data-stu-id="756e4-157">a.</span></span> <span data-ttu-id="756e4-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.peoplehr.com/`</span><span class="sxs-lookup"><span data-stu-id="756e4-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.peoplehr.com/`</span></span>

    <span data-ttu-id="756e4-159">b.</span><span class="sxs-lookup"><span data-stu-id="756e4-159">b.</span></span> <span data-ttu-id="756e4-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.peoplehr.com`</span><span class="sxs-lookup"><span data-stu-id="756e4-160">In the **Identifier** textbox, type a URL using the following pattern: `https://www.peoplehr.com`</span></span>

    <span data-ttu-id="756e4-161">c.</span><span class="sxs-lookup"><span data-stu-id="756e4-161">c.</span></span> <span data-ttu-id="756e4-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span><span class="sxs-lookup"><span data-stu-id="756e4-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="756e4-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="756e4-163">These values are not real.</span></span> <span data-ttu-id="756e4-164">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="756e4-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="756e4-165">Obraťte se na [tým podpory osoby klienta](mailto:customerservices@peoplehr.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="756e4-165">Contact [People Client support team](mailto:customerservices@peoplehr.com) to get these values.</span></span>

5. <span data-ttu-id="756e4-166">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="756e4-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_certificate.png) 

6. <span data-ttu-id="756e4-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="756e4-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="756e4-170">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte k přihlašování ke klientovi osoby jako správce.</span><span class="sxs-lookup"><span data-stu-id="756e4-170">To get SSO configured for your application, you need to sign-on to your People tenant as an administrator.</span></span>
   
8. <span data-ttu-id="756e4-171">V nabídce na levé straně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="756e4-171">In the menu on the left side, click **Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_001.png)

9. <span data-ttu-id="756e4-173">Klikněte na tlačítko **společnosti**.</span><span class="sxs-lookup"><span data-stu-id="756e4-173">Click **Company**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_002.png)

10. <span data-ttu-id="756e4-175">Na **nahrávání, jednotné přihlašování, SAML metadata souboru**, klikněte na tlačítko **Procházet** nahrát soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="756e4-175">On the **Upload 'Single Sign On' SAML meta-data file**, click **Browse** to upload the downloaded metadata file.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)

> [!TIP]
> <span data-ttu-id="756e4-177">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="756e4-177">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="756e4-178">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="756e4-178">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="756e4-179">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="756e4-179">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="756e4-180">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="756e4-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="756e4-181">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="756e4-181">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="756e4-183">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="756e4-183">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="756e4-184">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="756e4-184">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="756e4-186">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="756e4-186">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="756e4-188">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="756e4-188">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="756e4-190">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="756e4-190">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="756e4-192">a.</span><span class="sxs-lookup"><span data-stu-id="756e4-192">a.</span></span> <span data-ttu-id="756e4-193">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="756e4-193">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="756e4-194">b.</span><span class="sxs-lookup"><span data-stu-id="756e4-194">b.</span></span> <span data-ttu-id="756e4-195">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="756e4-195">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="756e4-196">c.</span><span class="sxs-lookup"><span data-stu-id="756e4-196">c.</span></span> <span data-ttu-id="756e4-197">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="756e4-197">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="756e4-198">d.</span><span class="sxs-lookup"><span data-stu-id="756e4-198">d.</span></span> <span data-ttu-id="756e4-199">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="756e4-199">Click **Create**.</span></span>
 
### <a name="creating-a-people-test-user"></a><span data-ttu-id="756e4-200">Vytvoření zkušebního uživatele osoby</span><span class="sxs-lookup"><span data-stu-id="756e4-200">Creating a People test user</span></span>

<span data-ttu-id="756e4-201">V této části vytvoříte uživatele volal Britta Simon v osoby.</span><span class="sxs-lookup"><span data-stu-id="756e4-201">In this section, you create a user called Britta Simon in People.</span></span> <span data-ttu-id="756e4-202">Práce s [tým podpory osoby klienta](mailto:customerservices@peoplehr.com) přidat uživatele do platformy osoby.</span><span class="sxs-lookup"><span data-stu-id="756e4-202">Work with [People Client support team](mailto:customerservices@peoplehr.com) to add the users in the People platform.</span></span> <span data-ttu-id="756e4-203">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="756e4-203">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="756e4-204">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="756e4-204">Assigning the Azure AD test user</span></span>

<span data-ttu-id="756e4-205">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup osobám.</span><span class="sxs-lookup"><span data-stu-id="756e4-205">In this section, you enable Britta Simon to use Azure single sign-on by granting access to People.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="756e4-207">**Přiřadit Britta Simon lidem, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="756e4-207">**To assign Britta Simon to People, perform the following steps:**</span></span>

1. <span data-ttu-id="756e4-208">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="756e4-208">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="756e4-210">V seznamu aplikací vyberte **osoby**.</span><span class="sxs-lookup"><span data-stu-id="756e4-210">In the applications list, select **People**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_app.png) 

3. <span data-ttu-id="756e4-212">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="756e4-212">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="756e4-214">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="756e4-214">Click **Add** button.</span></span> <span data-ttu-id="756e4-215">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="756e4-215">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="756e4-217">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="756e4-217">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="756e4-218">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="756e4-218">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="756e4-219">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="756e4-219">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="756e4-220">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="756e4-220">Testing single sign-on</span></span>

<span data-ttu-id="756e4-221">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="756e4-221">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="756e4-222">Když kliknete na dlaždici osoby na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci osoby.</span><span class="sxs-lookup"><span data-stu-id="756e4-222">When you click the People tile in the Access Panel, you should get automatically signed-on to your People application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="756e4-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="756e4-223">Additional resources</span></span>

* [<span data-ttu-id="756e4-224">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="756e4-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="756e4-225">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="756e4-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-people-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png

