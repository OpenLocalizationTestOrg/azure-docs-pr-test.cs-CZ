---
title: 'Kurz: Azure Active Directory integrace s Asset Bank | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Asset Bank."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3006ad6e-8831-41cd-94aa-7e7ae770ce7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 17bc0082e3721b50269cb4b17884c0e4a4cbcb5d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asset-bank"></a><span data-ttu-id="b08f9-103">Kurz: Azure Active Directory integrace s Asset Bank</span><span class="sxs-lookup"><span data-stu-id="b08f9-103">Tutorial: Azure Active Directory integration with Asset Bank</span></span>

<span data-ttu-id="b08f9-104">V tomto kurzu zjistěte, jak integrovat Asset Bank s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b08f9-104">In this tutorial, you learn how to integrate Asset Bank with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b08f9-105">Integrace Bank Asset s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b08f9-105">Integrating Asset Bank with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b08f9-106">Můžete řídit ve službě Azure AD, který má přístup k prostředku Bank</span><span class="sxs-lookup"><span data-stu-id="b08f9-106">You can control in Azure AD who has access to Asset Bank</span></span>
- <span data-ttu-id="b08f9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Asset Bank (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b08f9-107">You can enable your users to automatically get signed-on to Asset Bank (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b08f9-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b08f9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b08f9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b08f9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b08f9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b08f9-110">Prerequisites</span></span>

<span data-ttu-id="b08f9-111">Konfigurace integrace Azure AD s Asset Bank, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b08f9-111">To configure Azure AD integration with Asset Bank, you need the following items:</span></span>

- <span data-ttu-id="b08f9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b08f9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b08f9-113">Asset Bank jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b08f9-113">An Asset Bank single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b08f9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b08f9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b08f9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b08f9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b08f9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b08f9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b08f9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b08f9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b08f9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b08f9-118">Scenario description</span></span>
<span data-ttu-id="b08f9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b08f9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b08f9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b08f9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b08f9-121">Přidání Asset Bank z Galerie</span><span class="sxs-lookup"><span data-stu-id="b08f9-121">Adding Asset Bank from the gallery</span></span>
2. <span data-ttu-id="b08f9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b08f9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asset-bank-from-the-gallery"></a><span data-ttu-id="b08f9-123">Přidání Asset Bank z Galerie</span><span class="sxs-lookup"><span data-stu-id="b08f9-123">Adding Asset Bank from the gallery</span></span>
<span data-ttu-id="b08f9-124">Při konfiguraci integrace Asset Bank do služby Azure AD potřebujete přidat prostředek Bank z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b08f9-124">To configure the integration of Asset Bank into Azure AD, you need to add Asset Bank from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b08f9-125">**Chcete-li přidat prostředek Bank z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b08f9-125">**To add Asset Bank from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b08f9-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b08f9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b08f9-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b08f9-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b08f9-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b08f9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b08f9-133">Do vyhledávacího pole zadejte **Asset Bank**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-133">In the search box, type **Asset Bank**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_search.png)

5. <span data-ttu-id="b08f9-135">Na panelu výsledků vyberte **Asset Bank**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b08f9-135">In the results panel, select **Asset Bank**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b08f9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b08f9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b08f9-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Asset Bank podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b08f9-138">In this section, you configure and test Azure AD single sign-on with Asset Bank based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b08f9-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v bance Asset je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b08f9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Asset Bank is to a user in Azure AD.</span></span> <span data-ttu-id="b08f9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v bance Asset musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b08f9-140">In other words, a link relationship between an Azure AD user and the related user in Asset Bank needs to be established.</span></span>

<span data-ttu-id="b08f9-141">V bance Asset přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b08f9-141">In Asset Bank, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b08f9-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Asset Bank, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b08f9-142">To configure and test Azure AD single sign-on with Asset Bank, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b08f9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b08f9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b08f9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b08f9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b08f9-145">**[Vytváření testovacího uživatele Asset Bank](#creating-an-asset-bank-test-user)**  – Pokud chcete mít protějšek Britta Simon v bance Asset, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b08f9-145">**[Creating an Asset Bank test user](#creating-an-asset-bank-test-user)** - to have a counterpart of Britta Simon in Asset Bank that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b08f9-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b08f9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b08f9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b08f9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b08f9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b08f9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b08f9-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Asset Bank.</span><span class="sxs-lookup"><span data-stu-id="b08f9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Asset Bank application.</span></span>

<span data-ttu-id="b08f9-150">**Ke konfiguraci Azure AD jednotné přihlašování s Asset Bank, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b08f9-150">**To configure Azure AD single sign-on with Asset Bank, perform the following steps:**</span></span>

1. <span data-ttu-id="b08f9-151">Na portálu Azure na **Asset Bank** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-151">In the Azure portal, on the **Asset Bank** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b08f9-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b08f9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_samlbase.png)

3. <span data-ttu-id="b08f9-155">Na **Asset Bank domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b08f9-155">On the **Asset Bank Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_url.png)

    <span data-ttu-id="b08f9-157">a.</span><span class="sxs-lookup"><span data-stu-id="b08f9-157">a.</span></span> <span data-ttu-id="b08f9-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.assetbank-server.com`</span><span class="sxs-lookup"><span data-stu-id="b08f9-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.assetbank-server.com`</span></span>

    <span data-ttu-id="b08f9-159">b.</span><span class="sxs-lookup"><span data-stu-id="b08f9-159">b.</span></span> <span data-ttu-id="b08f9-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.assetbank-server.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="b08f9-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.assetbank-server.com/shibboleth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b08f9-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b08f9-161">These values are not real.</span></span> <span data-ttu-id="b08f9-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="b08f9-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b08f9-163">Obraťte se na [tým podpory Asset Bank klienta](mailto:support@assetbank.co.uk) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="b08f9-163">Contact [Asset Bank Client support team](mailto:support@assetbank.co.uk) to get these values.</span></span> 
 
4. <span data-ttu-id="b08f9-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b08f9-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_certificate.png) 

5. <span data-ttu-id="b08f9-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b08f9-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b08f9-168">Konfigurace jednotného přihlašování na **Asset Bank** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Asset Bank](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="b08f9-168">To configure single sign-on on **Asset Bank** side, you need to send the downloaded **Metadata XML** to [Asset Bank support team](mailto:support@assetbank.co.uk).</span></span> 


> [!TIP]
> <span data-ttu-id="b08f9-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b08f9-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b08f9-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b08f9-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b08f9-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b08f9-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b08f9-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b08f9-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="b08f9-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b08f9-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b08f9-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b08f9-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b08f9-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b08f9-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b08f9-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b08f9-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b08f9-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b08f9-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b08f9-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b08f9-184">a.</span><span class="sxs-lookup"><span data-stu-id="b08f9-184">a.</span></span> <span data-ttu-id="b08f9-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b08f9-186">b.</span><span class="sxs-lookup"><span data-stu-id="b08f9-186">b.</span></span> <span data-ttu-id="b08f9-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b08f9-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b08f9-188">c.</span><span class="sxs-lookup"><span data-stu-id="b08f9-188">c.</span></span> <span data-ttu-id="b08f9-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b08f9-190">d.</span><span class="sxs-lookup"><span data-stu-id="b08f9-190">d.</span></span> <span data-ttu-id="b08f9-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-191">Click **Create**.</span></span>
 
### <a name="creating-an-asset-bank-test-user"></a><span data-ttu-id="b08f9-192">Vytvoření Asset Bank testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b08f9-192">Creating an Asset Bank test user</span></span>

<span data-ttu-id="b08f9-193">Cílem této části je vytvoření uživatele volat Britta Simon v bance Asset.</span><span class="sxs-lookup"><span data-stu-id="b08f9-193">The objective of this section is to create a user called Britta Simon in Asset Bank.</span></span> <span data-ttu-id="b08f9-194">Asset Bank podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="b08f9-194">Asset Bank supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b08f9-195">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="b08f9-195">There is no action item for you in this section.</span></span> <span data-ttu-id="b08f9-196">Nový uživatel se vytvoří během pokusu o přístup k prostředku Bank, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="b08f9-196">A new user is created during an attempt to access Asset Bank if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="b08f9-197">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Asset Bank](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="b08f9-197">If you need to create a user manually, you need to contact the [Asset Bank support team](mailto:support@assetbank.co.uk).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b08f9-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b08f9-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b08f9-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k prostředku Bank.</span><span class="sxs-lookup"><span data-stu-id="b08f9-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Asset Bank.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b08f9-201">**Pokud chcete přiřadit Britta Simon Asset Bank, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b08f9-201">**To assign Britta Simon to Asset Bank, perform the following steps:**</span></span>

1. <span data-ttu-id="b08f9-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b08f9-204">V seznamu aplikací vyberte **Asset Bank**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-204">In the applications list, select **Asset Bank**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_app.png) 

3. <span data-ttu-id="b08f9-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b08f9-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b08f9-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b08f9-208">Click **Add** button.</span></span> <span data-ttu-id="b08f9-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b08f9-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b08f9-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b08f9-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b08f9-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b08f9-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b08f9-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b08f9-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b08f9-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b08f9-214">Testing single sign-on</span></span>

<span data-ttu-id="b08f9-215">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b08f9-215">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b08f9-216">Když kliknete na dlaždici Asset Bank na přístupovém panelu, jste měli získat automaticky přihlášení k vaší aplikaci pro bankovní Asset.</span><span class="sxs-lookup"><span data-stu-id="b08f9-216">When you click the Asset Bank tile in the Access Panel, you should get automatically signed-on to your Asset Bank application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b08f9-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b08f9-217">Additional resources</span></span>

* [<span data-ttu-id="b08f9-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b08f9-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b08f9-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b08f9-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_203.png

