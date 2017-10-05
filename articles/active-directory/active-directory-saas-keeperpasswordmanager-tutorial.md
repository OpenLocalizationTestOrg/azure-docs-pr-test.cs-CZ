---
title: "Kurz: Azure Active Directory integrace s správce hesel držitel & digitální trezoru | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a správce hesel držitel & digitální trezoru."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 36504a281756b980e3348e7f892ba08821873b52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a><span data-ttu-id="524cc-103">Kurz: Azure Active Directory integrace s správce hesel držitel & digitální trezoru</span><span class="sxs-lookup"><span data-stu-id="524cc-103">Tutorial: Azure Active Directory integration with Keeper Password Manager & Digital Vault</span></span>

<span data-ttu-id="524cc-104">V tomto kurzu zjistíte integrace správce hesel držitel & digitální trezor služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="524cc-104">In this tutorial, you learn how to integrate Keeper Password Manager & Digital Vault with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="524cc-105">Integrace správce hesel držitel & digitální trezoru s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="524cc-105">Integrating Keeper Password Manager & Digital Vault with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="524cc-106">Můžete řídit ve službě Azure AD, který má přístup k správce hesel držitel & digitální trezoru</span><span class="sxs-lookup"><span data-stu-id="524cc-106">You can control in Azure AD who has access to Keeper Password Manager & Digital Vault</span></span>
- <span data-ttu-id="524cc-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k správce hesel držitel & digitální trezoru (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="524cc-107">You can enable your users to automatically get signed-on to Keeper Password Manager & Digital Vault (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="524cc-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="524cc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="524cc-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="524cc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="524cc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="524cc-110">Prerequisites</span></span>

<span data-ttu-id="524cc-111">Chcete-li nakonfigurovat správce hesel držitel & digitální trezoru integrace Azure AD, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="524cc-111">To configure Azure AD integration with Keeper Password Manager & Digital Vault, you need the following items:</span></span>

- <span data-ttu-id="524cc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="524cc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="524cc-113">Správce hesel držitel & digitální trezoru jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="524cc-113">A Keeper Password Manager & Digital Vault single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="524cc-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="524cc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="524cc-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="524cc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="524cc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="524cc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="524cc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="524cc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="524cc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="524cc-118">Scenario description</span></span>
<span data-ttu-id="524cc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="524cc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="524cc-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="524cc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="524cc-121">Přidání správce hesel držitel & digitální trezoru z Galerie</span><span class="sxs-lookup"><span data-stu-id="524cc-121">Adding Keeper Password Manager & Digital Vault from the gallery</span></span>
2. <span data-ttu-id="524cc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="524cc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a><span data-ttu-id="524cc-123">Přidání správce hesel držitel & digitální trezoru z Galerie</span><span class="sxs-lookup"><span data-stu-id="524cc-123">Adding Keeper Password Manager & Digital Vault from the gallery</span></span>
<span data-ttu-id="524cc-124">Při konfiguraci integrace správce hesel držitel & digitální trezoru do služby Azure AD, musíte přidat správce hesel držitel & digitální trezoru z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="524cc-124">To configure the integration of Keeper Password Manager & Digital Vault into Azure AD, you need to add Keeper Password Manager & Digital Vault from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="524cc-125">**Chcete-li přidat správce hesel držitel & digitální trezoru z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="524cc-125">**To add Keeper Password Manager & Digital Vault from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="524cc-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="524cc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="524cc-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="524cc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="524cc-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="524cc-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="524cc-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="524cc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="524cc-133">Do vyhledávacího pole zadejte **správce hesel držitel & digitální trezoru**.</span><span class="sxs-lookup"><span data-stu-id="524cc-133">In the search box, type **Keeper Password Manager & Digital Vault**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. <span data-ttu-id="524cc-135">Na panelu výsledků vyberte **správce hesel držitel & digitální trezoru**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="524cc-135">In the results panel, select **Keeper Password Manager & Digital Vault**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="524cc-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="524cc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="524cc-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s správce hesel držitel & digitální trezoru podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="524cc-138">In this section, you configure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="524cc-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v správce hesel držitel & digitální trezoru je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="524cc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Keeper Password Manager & Digital Vault is to a user in Azure AD.</span></span> <span data-ttu-id="524cc-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské správce hesel držitel & digitální trezoru.</span><span class="sxs-lookup"><span data-stu-id="524cc-140">In other words, a link relationship between an Azure AD user and the related user in Keeper Password Manager & Digital Vault needs to be established.</span></span>

<span data-ttu-id="524cc-141">V & digitální trezoru správce hesel držitel, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="524cc-141">In Keeper Password Manager & Digital Vault, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="524cc-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s správce hesel držitel & digitální trezoru, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="524cc-142">To configure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="524cc-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="524cc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="524cc-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="524cc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="524cc-145">**[Vytvoření zkušebního uživatele správce hesel držitel & digitální trezoru](#creating-a-keeperpasswordmanager-test-user)**  – Pokud chcete mít protějšek Britta Simon správce hesel držitel & digitální trezoru, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="524cc-145">**[Creating a Keeper Password Manager & Digital Vault test user](#creating-a-keeperpasswordmanager-test-user)** - to have a counterpart of Britta Simon in Keeper Password Manager & Digital Vault that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="524cc-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="524cc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="524cc-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="524cc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="524cc-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="524cc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="524cc-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci správce hesel držitel & digitální trezoru.</span><span class="sxs-lookup"><span data-stu-id="524cc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Keeper Password Manager & Digital Vault application.</span></span>

<span data-ttu-id="524cc-150">**Ke konfiguraci Azure AD jednotné přihlašování s správce hesel držitel & digitální trezoru, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="524cc-150">**To configure Azure AD single sign-on with Keeper Password Manager & Digital Vault, perform the following steps:**</span></span>

1. <span data-ttu-id="524cc-151">Na portálu Azure na **správce hesel držitel & digitální trezoru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="524cc-151">In the Azure portal, on the **Keeper Password Manager & Digital Vault** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="524cc-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="524cc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. <span data-ttu-id="524cc-155">Na **správce hesel držitel & digitální trezoru domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="524cc-155">On the **Keeper Password Manager & Digital Vault Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    <span data-ttu-id="524cc-157">a.</span><span class="sxs-lookup"><span data-stu-id="524cc-157">a.</span></span> <span data-ttu-id="524cc-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span><span class="sxs-lookup"><span data-stu-id="524cc-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span></span>

    <span data-ttu-id="524cc-159">b.</span><span class="sxs-lookup"><span data-stu-id="524cc-159">b.</span></span> <span data-ttu-id="524cc-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="524cc-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span></span>

    <span data-ttu-id="524cc-161">c.</span><span class="sxs-lookup"><span data-stu-id="524cc-161">c.</span></span> <span data-ttu-id="524cc-162">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://{SSO CONNECT SERVER}/sso-connect`</span><span class="sxs-lookup"><span data-stu-id="524cc-162">In the **Identifier** textbox, type a URL using the following pattern: `https://{SSO CONNECT SERVER}/sso-connect`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="524cc-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="524cc-163">These values are not real.</span></span> <span data-ttu-id="524cc-164">Tyto hodnoty aktualizujte s skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="524cc-164">Update these values with the actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="524cc-165">Obraťte se na [správce hesel držitel & digitální trezoru podporu klientů team](https://keepersecurity.com/contact.html) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="524cc-165">Contact [Keeper Password Manager & Digital Vault Client support team](https://keepersecurity.com/contact.html) to get these values.</span></span> 

4. <span data-ttu-id="524cc-166">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="524cc-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. <span data-ttu-id="524cc-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="524cc-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="524cc-170">Na **správce hesel držitel & digitální konfigurace trezoru** klikněte na tlačítko **nakonfigurovat správce hesel držitel & digitální trezoru** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="524cc-170">On the **Keeper Password Manager & Digital Vault Configuration** section, click **Configure Keeper Password Manager & Digital Vault** to open **Configure sign-on** window.</span></span> <span data-ttu-id="524cc-171">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="524cc-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. <span data-ttu-id="524cc-173">Konfigurace jednotného přihlašování na **správce hesel držitel & digitální konfigurace trezoru** straně, postupujte podle pokynů uveden na [držitel podpora Průvodce](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span><span class="sxs-lookup"><span data-stu-id="524cc-173">To configure single sign-on on **Keeper Password Manager & Digital Vault Configuration** side, follow the guidelines given at [Keeper Support Guide](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span></span>

> [!TIP]
> <span data-ttu-id="524cc-174">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="524cc-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="524cc-175">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="524cc-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="524cc-176">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="524cc-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="524cc-177">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="524cc-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="524cc-178">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="524cc-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="524cc-180">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="524cc-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="524cc-181">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="524cc-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="524cc-183">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="524cc-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="524cc-185">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="524cc-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="524cc-187">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="524cc-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="524cc-189">a.</span><span class="sxs-lookup"><span data-stu-id="524cc-189">a.</span></span> <span data-ttu-id="524cc-190">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="524cc-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="524cc-191">b.</span><span class="sxs-lookup"><span data-stu-id="524cc-191">b.</span></span> <span data-ttu-id="524cc-192">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="524cc-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="524cc-193">c.</span><span class="sxs-lookup"><span data-stu-id="524cc-193">c.</span></span> <span data-ttu-id="524cc-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="524cc-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="524cc-195">d.</span><span class="sxs-lookup"><span data-stu-id="524cc-195">d.</span></span> <span data-ttu-id="524cc-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="524cc-196">Click **Create**.</span></span>
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a><span data-ttu-id="524cc-197">Vytvoření zkušebního uživatele správce hesel držitel & digitální trezoru</span><span class="sxs-lookup"><span data-stu-id="524cc-197">Creating a Keeper Password Manager & Digital Vault test user</span></span>

<span data-ttu-id="524cc-198">Pokud chcete povolit uživatelům Azure AD přihlášení do správce hesel držitel & digitální trezoru, musí být zřízená do správce hesel držitel & digitální trezoru.</span><span class="sxs-lookup"><span data-stu-id="524cc-198">To enable Azure AD users to log in to Keeper Password Manager & Digital Vault, they must be provisioned into Keeper Password Manager & Digital Vault.</span></span> <span data-ttu-id="524cc-199">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele automaticky se vytvoří v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="524cc-199">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> <span data-ttu-id="524cc-200">Obraťte se na [držitel podporu](https://keepersecurity.com/contact.html), pokud chcete nastavit uživatele ručně.</span><span class="sxs-lookup"><span data-stu-id="524cc-200">You can contact [Keeper Support](https://keepersecurity.com/contact.html), if you want to setup users manually.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="524cc-201">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="524cc-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="524cc-202">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu správce hesel držitel & digitální trezoru.</span><span class="sxs-lookup"><span data-stu-id="524cc-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Keeper Password Manager & Digital Vault.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="524cc-204">**Pokud chcete přiřadit Britta Simon správce hesel držitel & digitální trezoru, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="524cc-204">**To assign Britta Simon to Keeper Password Manager & Digital Vault, perform the following steps:**</span></span>

1. <span data-ttu-id="524cc-205">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="524cc-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="524cc-207">V seznamu aplikací vyberte **správce hesel držitel & digitální trezoru**.</span><span class="sxs-lookup"><span data-stu-id="524cc-207">In the applications list, select **Keeper Password Manager & Digital Vault**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. <span data-ttu-id="524cc-209">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="524cc-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="524cc-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="524cc-211">Click **Add** button.</span></span> <span data-ttu-id="524cc-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="524cc-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="524cc-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="524cc-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="524cc-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="524cc-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="524cc-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="524cc-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="524cc-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="524cc-217">Testing single sign-on</span></span>

<span data-ttu-id="524cc-218">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="524cc-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="524cc-219">Když kliknete na dlaždici správce hesel držitel & digitální trezoru na přístupovém panelu, měli byste obdržet přihlašovací stránku aplikace správce hesel držitel & digitální trezoru.</span><span class="sxs-lookup"><span data-stu-id="524cc-219">When you click the Keeper Password Manager & Digital Vault tile in the Access Panel, you should get login page of Keeper Password Manager & Digital Vault application.</span></span> <span data-ttu-id="524cc-220">Po úspěšném ověření měli byste obdržet do aplikace.</span><span class="sxs-lookup"><span data-stu-id="524cc-220">Upon successful authentication, you should get into the application.</span></span> <span data-ttu-id="524cc-221">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="524cc-221">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="524cc-222">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="524cc-222">Additional resources</span></span>

* [<span data-ttu-id="524cc-223">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="524cc-223">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="524cc-224">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="524cc-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_203.png

