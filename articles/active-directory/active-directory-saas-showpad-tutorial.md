---
title: 'Kurz: Azure Active Directory integrace s Showpad | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Showpad."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48b6bee0-dbc5-4863-964d-75b25e517741
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: c8b39c9215675d8073f896f934339e7cd55334cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-showpad"></a><span data-ttu-id="8ff8d-103">Kurz: Azure Active Directory integrace s Showpad</span><span class="sxs-lookup"><span data-stu-id="8ff8d-103">Tutorial: Azure Active Directory integration with Showpad</span></span>

<span data-ttu-id="8ff8d-104">V tomto kurzu zjistěte, jak integrovat Showpad s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8ff8d-104">In this tutorial, you learn how to integrate Showpad with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ff8d-105">Integrace Showpad s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-105">Integrating Showpad with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8ff8d-106">Můžete řídit ve službě Azure AD, který má přístup k Showpad</span><span class="sxs-lookup"><span data-stu-id="8ff8d-106">You can control in Azure AD who has access to Showpad</span></span>
- <span data-ttu-id="8ff8d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Showpad (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ff8d-107">You can enable your users to automatically get signed-on to Showpad (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ff8d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8ff8d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8ff8d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ff8d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ff8d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8ff8d-110">Prerequisites</span></span>

<span data-ttu-id="8ff8d-111">Konfigurace integrace Azure AD s Showpad, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-111">To configure Azure AD integration with Showpad, you need the following items:</span></span>

- <span data-ttu-id="8ff8d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ff8d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8ff8d-113">Showpad jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8ff8d-113">A Showpad single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8ff8d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8ff8d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ff8d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8ff8d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ff8d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8ff8d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8ff8d-118">Scenario description</span></span>
<span data-ttu-id="8ff8d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ff8d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ff8d-121">Přidání Showpad z Galerie</span><span class="sxs-lookup"><span data-stu-id="8ff8d-121">Adding Showpad from the gallery</span></span>
2. <span data-ttu-id="8ff8d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ff8d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-showpad-from-the-gallery"></a><span data-ttu-id="8ff8d-123">Přidání Showpad z Galerie</span><span class="sxs-lookup"><span data-stu-id="8ff8d-123">Adding Showpad from the gallery</span></span>

<span data-ttu-id="8ff8d-124">Při konfiguraci integrace Showpad do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Showpad z galerie.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-124">To configure the integration of Showpad into Azure AD, you need to add Showpad from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8ff8d-125">**Pokud chcete přidat Showpad z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ff8d-125">**To add Showpad from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff8d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ff8d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8ff8d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8ff8d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8ff8d-133">Do vyhledávacího pole zadejte **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-133">In the search box, type **Showpad**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_search.png)

5. <span data-ttu-id="8ff8d-135">Na panelu výsledků vyberte **Showpad**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-135">In the results panel, select **Showpad**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ff8d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ff8d-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="8ff8d-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Showpad podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8ff8d-138">In this section, you configure and test Azure AD single sign-on with Showpad based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8ff8d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Showpad je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Showpad is to a user in Azure AD.</span></span> <span data-ttu-id="8ff8d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Showpad musí navázat.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-140">In other words, a link relationship between an Azure AD user and the related user in Showpad needs to be established.</span></span>

<span data-ttu-id="8ff8d-141">V Showpad, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-141">In Showpad, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8ff8d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Showpad, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-142">To configure and test Azure AD single sign-on with Showpad, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8ff8d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8ff8d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ff8d-145">**[Vytvoření zkušebního uživatele Showpad](#creating-a-showpad-test-user)**  – Pokud chcete mít protějšek Britta Simon v Showpad propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-145">**[Creating a Showpad test user](#creating-a-showpad-test-user)** - to have a counterpart of Britta Simon in Showpad that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8ff8d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ff8d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ff8d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ff8d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ff8d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Showpad.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Showpad application.</span></span>

<span data-ttu-id="8ff8d-150">**Ke konfiguraci Azure AD jednotné přihlašování s Showpad, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ff8d-150">**To configure Azure AD single sign-on with Showpad, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff8d-151">Na portálu Azure na **Showpad** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-151">In the Azure portal, on the **Showpad** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8ff8d-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_samlbase.png)

3. <span data-ttu-id="8ff8d-155">Na **Showpad domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-155">On the **Showpad Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_url.png)

    <span data-ttu-id="8ff8d-157">a.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-157">a.</span></span> <span data-ttu-id="8ff8d-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<comapany-name>.showpad.biz/login`</span><span class="sxs-lookup"><span data-stu-id="8ff8d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<comapany-name>.showpad.biz/login`</span></span>

    <span data-ttu-id="8ff8d-159">b.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-159">b.</span></span> <span data-ttu-id="8ff8d-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company-name>.showpad.biz`</span><span class="sxs-lookup"><span data-stu-id="8ff8d-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company-name>.showpad.biz`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8ff8d-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-161">These values are not real.</span></span> <span data-ttu-id="8ff8d-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8ff8d-163">Obraťte se na [tým podpory Showpad](https://help.showpad.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-163">Contact [Showpad support team](https://help.showpad.com) to get these values.</span></span> 
 


4. <span data-ttu-id="8ff8d-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_certificate.png) 

5. <span data-ttu-id="8ff8d-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8ff8d-168">Přihlášení ke klientovi Showpad jako správce.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-168">Sign-on to your Showpad tenant as an administrator.</span></span>

7. <span data-ttu-id="8ff8d-169">V nabídce v horní části, klikněte **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-169">In the menu on the top, click the **Settings**.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

8. <span data-ttu-id="8ff8d-171">Přejděte na "**jednotné přihlašování**"a klikněte na tlačítko"**povolit**."</span><span class="sxs-lookup"><span data-stu-id="8ff8d-171">Navigate to "**Single Sign-On**" and click "**Enable**."</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

9. <span data-ttu-id="8ff8d-173">Na **přidat službu SAML 2.0** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-173">On the **Add a SAML 2.0 Service** dialog, perform the following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 
   
    <span data-ttu-id="8ff8d-175">a.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-175">a.</span></span> <span data-ttu-id="8ff8d-176">V **název** textovému poli, zadejte název zprostředkovatele identifikátor (například: název vaší společnosti).</span><span class="sxs-lookup"><span data-stu-id="8ff8d-176">In the **Name** textbox, type the name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="8ff8d-177">b.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-177">b.</span></span> <span data-ttu-id="8ff8d-178">Jako **Metadata zdroje**, vyberte **XML**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-178">As **Metadata Source**, select **XML**.</span></span>
   
    <span data-ttu-id="8ff8d-179">c.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-179">c.</span></span> <span data-ttu-id="8ff8d-180">Obsah souboru XML metadata, která jste si stáhli z portálu Azure, zkopírujte a vložte ji do **soubor XML s metadaty** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-180">Copy the content of metadata XML file, which you have downloaded from the Azure portal, and then paste it into the **Metadata XML** textbox.</span></span>
   
    <span data-ttu-id="8ff8d-181">d.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-181">d.</span></span> <span data-ttu-id="8ff8d-182">Vyberte **automatického zřizování účtů pro nové uživatele při přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-182">Select **Auto-provision accounts for new users when they log in**.</span></span>
   
    <span data-ttu-id="8ff8d-183">e.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-183">e.</span></span> <span data-ttu-id="8ff8d-184">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-184">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="8ff8d-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="8ff8d-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8ff8d-186">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8ff8d-187">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8ff8d-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ff8d-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ff8d-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ff8d-189">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8ff8d-191">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ff8d-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff8d-192">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ff8d-194">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ff8d-196">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ff8d-198">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ff8d-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ff8d-200">a.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-200">a.</span></span> <span data-ttu-id="8ff8d-201">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ff8d-202">b.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-202">b.</span></span> <span data-ttu-id="8ff8d-203">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ff8d-204">c.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-204">c.</span></span> <span data-ttu-id="8ff8d-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8ff8d-206">d.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-206">d.</span></span> <span data-ttu-id="8ff8d-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-207">Click **Create**.</span></span>
 
### <a name="creating-a-showpad-test-user"></a><span data-ttu-id="8ff8d-208">Vytvoření zkušebního uživatele Showpad</span><span class="sxs-lookup"><span data-stu-id="8ff8d-208">Creating a Showpad test user</span></span>

<span data-ttu-id="8ff8d-209">Cílem této části je vytvoření uživatele v Showpad nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-209">The objective of this section is to create a user called Britta Simon in Showpad.</span></span> 

<span data-ttu-id="8ff8d-210">Showpad podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-210">Showpad supports just-in-time provisioning.</span></span> <span data-ttu-id="8ff8d-211">Povolení zřizování v  **[konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-211">You have enabled provisioning in **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**.</span></span> 

<span data-ttu-id="8ff8d-212">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-212">There is no action item for you in this section.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8ff8d-213">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ff8d-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8ff8d-214">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Showpad.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Showpad.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8ff8d-216">**Pokud chcete přiřadit Britta Simon Showpad, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ff8d-216">**To assign Britta Simon to Showpad, perform the following steps:**</span></span>

1. <span data-ttu-id="8ff8d-217">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8ff8d-219">V seznamu aplikací vyberte **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-219">In the applications list, select **Showpad**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_app.png) 

3. <span data-ttu-id="8ff8d-221">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8ff8d-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-223">Click **Add** button.</span></span> <span data-ttu-id="8ff8d-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8ff8d-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8ff8d-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ff8d-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8ff8d-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ff8d-229">Testing single sign-on</span></span>

<span data-ttu-id="8ff8d-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8ff8d-231">Když kliknete na dlaždici Showpad na přístupovém panelu, jste měli získat automaticky přihlášení k Showpad aplikace.</span><span class="sxs-lookup"><span data-stu-id="8ff8d-231">When you click the Showpad tile in the Access Panel, you should get automatically signed-on to Showpad application.</span></span>
<span data-ttu-id="8ff8d-232">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8ff8d-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ff8d-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8ff8d-233">Additional resources</span></span>

* [<span data-ttu-id="8ff8d-234">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8ff8d-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ff8d-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8ff8d-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png

