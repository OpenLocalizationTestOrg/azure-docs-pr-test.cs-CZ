---
title: 'Kurz: Azure Active Directory integrace s Overdrive | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Overdrive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e68cede7-1130-4813-bd55-22a9a6fc6bf5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 515dd397c46df7c8c82afab9b50051e34db69d7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-overdrive"></a><span data-ttu-id="a4b74-103">Kurz: Azure Active Directory integrace s Overdrive</span><span class="sxs-lookup"><span data-stu-id="a4b74-103">Tutorial: Azure Active Directory integration with Overdrive</span></span> 

<span data-ttu-id="a4b74-104">V tomto kurzu zjistěte, jak integrovat Overdrive s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a4b74-104">In this tutorial, you learn how to integrate Overdrive with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a4b74-105">Integrace Overdrive s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a4b74-105">Integrating Overdrive with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a4b74-106">Můžete řídit ve službě Azure AD, který má přístup k Overdrive</span><span class="sxs-lookup"><span data-stu-id="a4b74-106">You can control in Azure AD who has access to Overdrive</span></span> 
- <span data-ttu-id="a4b74-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Overdrive (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4b74-107">You can enable your users to automatically get signed-on to Overdrive (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a4b74-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a4b74-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a4b74-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a4b74-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4b74-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4b74-110">Prerequisites</span></span>

<span data-ttu-id="a4b74-111">Konfigurace integrace Azure AD s Overdrive, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a4b74-111">To configure Azure AD integration with Overdrive, you need the following items:</span></span>

- <span data-ttu-id="a4b74-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4b74-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a4b74-113">Overdrive jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a4b74-113">An Overdrive single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a4b74-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4b74-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a4b74-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a4b74-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a4b74-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a4b74-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a4b74-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a4b74-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a4b74-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a4b74-118">Scenario description</span></span>
<span data-ttu-id="a4b74-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4b74-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a4b74-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a4b74-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a4b74-121">Přidání Overdrive z Galerie</span><span class="sxs-lookup"><span data-stu-id="a4b74-121">Adding Overdrive from the gallery</span></span>
2. <span data-ttu-id="a4b74-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b74-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-overdrive-from-the-gallery"></a><span data-ttu-id="a4b74-123">Přidání Overdrive z Galerie</span><span class="sxs-lookup"><span data-stu-id="a4b74-123">Adding Overdrive from the gallery</span></span>
<span data-ttu-id="a4b74-124">Chcete-li nakonfigurovat integraci Overdrive do služby Azure AD, přidejte Overdrive z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a4b74-124">To configure the integration of Overdrive into Azure AD, you need to add Overdrive from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a4b74-125">**Pokud chcete přidat Overdrive z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4b74-125">**To add Overdrive from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a4b74-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4b74-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a4b74-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a4b74-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a4b74-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b74-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a4b74-133">Do vyhledávacího pole zadejte **Overdrive**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-133">In the search box, type **Overdrive**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_search.png)

5. <span data-ttu-id="a4b74-135">Na panelu výsledků vyberte **Overdrive**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4b74-135">In the results panel, select **Overdrive**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a4b74-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b74-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a4b74-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Overdrive podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a4b74-138">In this section, you configure and test Azure AD single sign-on with Overdrive based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a4b74-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Overdrive je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a4b74-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Overdrive is to a user in Azure AD.</span></span> <span data-ttu-id="a4b74-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Overdrive musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a4b74-140">In other words, a link relationship between an Azure AD user and the related user in Overdrive needs to be established.</span></span>

<span data-ttu-id="a4b74-141">V Overdrive, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a4b74-141">In Overdrive, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a4b74-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Overdrive, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a4b74-142">To configure and test Azure AD single sign-on with Overdrive, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a4b74-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a4b74-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a4b74-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4b74-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a4b74-145">**[Vytváření testovacího uživatele Overdrive](#creating-an-overdrive-test-user)**  – Pokud chcete mít protějšek Britta Simon v Overdrive propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4b74-145">**[Creating an Overdrive test user](#creating-an-overdrive-test-user)** - to have a counterpart of Britta Simon in Overdrive that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a4b74-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4b74-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a4b74-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a4b74-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a4b74-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b74-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a4b74-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Overdrive.</span><span class="sxs-lookup"><span data-stu-id="a4b74-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Overdrive  application.</span></span>

<span data-ttu-id="a4b74-150">**Ke konfiguraci Azure AD jednotné přihlašování s Overdrive, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4b74-150">**To configure Azure AD single sign-on with Overdrive, perform the following steps:**</span></span>

1. <span data-ttu-id="a4b74-151">Na portálu Azure na **Overdrive** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-151">In the Azure portal, on the **Overdrive** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a4b74-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4b74-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_samlbase.png)

3. <span data-ttu-id="a4b74-155">Na **Overdrive domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4b74-155">On the **Overdrive Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_url.png)

    <span data-ttu-id="a4b74-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://<subdomain>.libraryreserve.com`</span><span class="sxs-lookup"><span data-stu-id="a4b74-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<subdomain>.libraryreserve.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a4b74-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="a4b74-158">The value is not real.</span></span> <span data-ttu-id="a4b74-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4b74-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="a4b74-160">Obraťte se na [tým podpory Overdrive klienta](https://help.overdrive.com/) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a4b74-160">Contact [Overdrive Client support team](https://help.overdrive.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="a4b74-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a4b74-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_certificate.png) 

5. <span data-ttu-id="a4b74-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4b74-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a4b74-165">Konfigurace jednotného přihlašování na **Overdrive** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Overdrive](https://help.overdrive.com/).</span><span class="sxs-lookup"><span data-stu-id="a4b74-165">To configure single sign-on on **Overdrive** side, you need to send the downloaded **Metadata XML** to [Overdrive support team](https://help.overdrive.com/).</span></span> <span data-ttu-id="a4b74-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="a4b74-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="a4b74-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a4b74-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a4b74-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a4b74-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a4b74-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a4b74-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a4b74-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4b74-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="a4b74-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4b74-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a4b74-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4b74-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a4b74-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4b74-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a4b74-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a4b74-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b74-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a4b74-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4b74-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a4b74-182">a.</span><span class="sxs-lookup"><span data-stu-id="a4b74-182">a.</span></span> <span data-ttu-id="a4b74-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a4b74-184">b.</span><span class="sxs-lookup"><span data-stu-id="a4b74-184">b.</span></span> <span data-ttu-id="a4b74-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a4b74-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a4b74-186">c.</span><span class="sxs-lookup"><span data-stu-id="a4b74-186">c.</span></span> <span data-ttu-id="a4b74-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a4b74-188">d.</span><span class="sxs-lookup"><span data-stu-id="a4b74-188">d.</span></span> <span data-ttu-id="a4b74-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-189">Click **Create**.</span></span>
 
### <a name="creating-an-overdrive-test-user"></a><span data-ttu-id="a4b74-190">Vytvoření Overdrive testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a4b74-190">Creating an Overdrive test user</span></span>

<span data-ttu-id="a4b74-191">Neexistuje žádná položka akce můžete nakonfigurovat na OverDrive zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a4b74-191">There is no action item for you to configure user provisioning to OverDrive.</span></span>  

<span data-ttu-id="a4b74-192">Když přiřazený uživatel se pokusí přihlásit k OverDrive, účet OverDrive se automaticky vytvoří v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a4b74-192">When an assigned user tries to log in to OverDrive, an OverDrive account is automatically created if necessary.</span></span>

>[!NOTE]
><span data-ttu-id="a4b74-193">Můžete použít všechny ostatní OverDrive uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované OverDrive zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a4b74-193">You can use any other OverDrive user account creation tools or APIs provided by OverDrive to provision AAD user accounts.</span></span>
>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a4b74-194">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4b74-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a4b74-195">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Overdrive.</span><span class="sxs-lookup"><span data-stu-id="a4b74-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Overdrive.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a4b74-197">**Pokud chcete přiřadit Britta Simon Overdrive, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4b74-197">**To assign Britta Simon to Overdrive, perform the following steps:**</span></span>

1. <span data-ttu-id="a4b74-198">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a4b74-200">V seznamu aplikací vyberte **Overdrive**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-200">In the applications list, select **Overdrive**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_app.png) 

3. <span data-ttu-id="a4b74-202">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a4b74-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a4b74-204">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4b74-204">Click **Add** button.</span></span> <span data-ttu-id="a4b74-205">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b74-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a4b74-207">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a4b74-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a4b74-208">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b74-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a4b74-209">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b74-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a4b74-210">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b74-210">Testing single sign-on</span></span>

<span data-ttu-id="a4b74-211">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a4b74-211">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a4b74-212">Když kliknete na dlaždici Overdrive na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Overdrive.</span><span class="sxs-lookup"><span data-stu-id="a4b74-212">When you click the Overdrive tile in the Access Panel, you should get automatically signed-on to your Overdrive application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4b74-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a4b74-213">Additional resources</span></span>

* [<span data-ttu-id="a4b74-214">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a4b74-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a4b74-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a4b74-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_203.png

