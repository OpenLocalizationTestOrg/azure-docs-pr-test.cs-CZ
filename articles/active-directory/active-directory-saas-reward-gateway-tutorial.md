---
title: "Kurz: Azure Active Directory integrace s bránou potřebu | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a účet brány."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34336386-998a-4d47-ab55-721d97708e5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: b1a468caa22159ad603dbec1ef530e7e0e24f96d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-reward-gateway"></a><span data-ttu-id="e8afd-103">Kurz: Azure Active Directory integrace s potřebu brány</span><span class="sxs-lookup"><span data-stu-id="e8afd-103">Tutorial: Azure Active Directory integration with Reward Gateway</span></span>

<span data-ttu-id="e8afd-104">V tomto kurzu zjistěte, jak integrovat potřebu brány s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e8afd-104">In this tutorial, you learn how to integrate Reward Gateway with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e8afd-105">Integrace brány potřebu s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e8afd-105">Integrating Reward Gateway with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e8afd-106">Můžete řídit ve službě Azure AD, který má přístup k bráně potřebu</span><span class="sxs-lookup"><span data-stu-id="e8afd-106">You can control in Azure AD who has access to Reward Gateway</span></span>
- <span data-ttu-id="e8afd-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k bráně potřebu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e8afd-107">You can enable your users to automatically get signed-on to Reward Gateway (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e8afd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e8afd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e8afd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e8afd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8afd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e8afd-110">Prerequisites</span></span>

<span data-ttu-id="e8afd-111">Ke konfiguraci integrace služby Azure AD s bránou potřebu, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e8afd-111">To configure Azure AD integration with Reward Gateway, you need the following items:</span></span>

- <span data-ttu-id="e8afd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e8afd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e8afd-113">Potřebu brány jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e8afd-113">A Reward Gateway single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e8afd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e8afd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e8afd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e8afd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e8afd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e8afd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e8afd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e8afd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e8afd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e8afd-118">Scenario description</span></span>
<span data-ttu-id="e8afd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e8afd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e8afd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e8afd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e8afd-121">Přidání brány potřebu z Galerie</span><span class="sxs-lookup"><span data-stu-id="e8afd-121">Adding Reward Gateway from the gallery</span></span>
2. <span data-ttu-id="e8afd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8afd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-reward-gateway-from-the-gallery"></a><span data-ttu-id="e8afd-123">Přidání brány potřebu z Galerie</span><span class="sxs-lookup"><span data-stu-id="e8afd-123">Adding Reward Gateway from the gallery</span></span>
<span data-ttu-id="e8afd-124">Při konfiguraci integrace potřebu brány do služby Azure AD, potřebujete přidat účet brány z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e8afd-124">To configure the integration of Reward Gateway into Azure AD, you need to add Reward Gateway from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e8afd-125">**Chcete-li přidat bránu potřebu z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e8afd-125">**To add Reward Gateway from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e8afd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e8afd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e8afd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e8afd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e8afd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8afd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e8afd-133">Do vyhledávacího pole zadejte **potřebu brány**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-133">In the search box, type **Reward Gateway**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_search.png)

5. <span data-ttu-id="e8afd-135">Na panelu výsledků vyberte **potřebu brány**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e8afd-135">In the results panel, select **Reward Gateway**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e8afd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8afd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e8afd-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s potřebu brány podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e8afd-138">In this section, you configure and test Azure AD single sign-on with Reward Gateway based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e8afd-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v bráně potřebu je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e8afd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Reward Gateway is to a user in Azure AD.</span></span> <span data-ttu-id="e8afd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské potřebu brány je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e8afd-140">In other words, a link relationship between an Azure AD user and the related user in Reward Gateway needs to be established.</span></span>

<span data-ttu-id="e8afd-141">V bráně potřebu přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e8afd-141">In Reward Gateway, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e8afd-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s potřebu brány, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e8afd-142">To configure and test Azure AD single sign-on with Reward Gateway, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e8afd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e8afd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e8afd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e8afd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e8afd-145">**[Vytvoření zkušebního uživatele potřebu brány](#creating-a-reward-gateway-test-user)**  – Pokud chcete mít protějšek Britta Simon v potřebu brány, kterou je propojena k reprezentaci Azure AD uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8afd-145">**[Creating a Reward Gateway test user](#creating-a-reward-gateway-test-user)** - to have a counterpart of Britta Simon in Reward Gateway that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e8afd-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e8afd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e8afd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e8afd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e8afd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8afd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e8afd-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci potřebu brány.</span><span class="sxs-lookup"><span data-stu-id="e8afd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Reward Gateway application.</span></span>

<span data-ttu-id="e8afd-150">**Ke konfiguraci Azure AD jednotné přihlašování s bránou potřebu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e8afd-150">**To configure Azure AD single sign-on with Reward Gateway, perform the following steps:**</span></span>

1. <span data-ttu-id="e8afd-151">Na portálu Azure na **potřebu brány** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-151">In the Azure portal, on the **Reward Gateway** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e8afd-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e8afd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_samlbase.png)

3. <span data-ttu-id="e8afd-155">Na **potřebu brány domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e8afd-155">On the **Reward Gateway Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_url.png)

    <span data-ttu-id="e8afd-157">a.</span><span class="sxs-lookup"><span data-stu-id="e8afd-157">a.</span></span> <span data-ttu-id="e8afd-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="e8afd-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.rewardgateway.com` |
    | `https://<companyname>.rewardgateway.co.uk/` |
    | `https://<companyname>.rewardgateway.co.nz/` |
    | `https://<companyname>.rewardgateway.com.au/` |

    <span data-ttu-id="e8afd-159">b.</span><span class="sxs-lookup"><span data-stu-id="e8afd-159">b.</span></span> <span data-ttu-id="e8afd-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="e8afd-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    |  `https://<companyname>.rewardgateway.com/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.co.uk/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.co.nz/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.com.au/Authentication/EndLogin?idp=<Unique Id>` |

    > [!NOTE] 
    > <span data-ttu-id="e8afd-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e8afd-161">These values are not real.</span></span> <span data-ttu-id="e8afd-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e8afd-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="e8afd-163">Obraťte se na [tým podpory brány potřebu](mailto:clientsupport@rewardgateway.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e8afd-163">Contact [Reward Gateway support team](mailto:clientsupport@rewardgateway.com) to get these values.</span></span>
 
4. <span data-ttu-id="e8afd-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e8afd-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_certificate.png) 

5. <span data-ttu-id="e8afd-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e8afd-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e8afd-168">Konfigurace jednotného přihlašování na **potřebu brány** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory brány potřebu](mailto:clientsupport@rewardgateway.com).</span><span class="sxs-lookup"><span data-stu-id="e8afd-168">To configure single sign-on on **Reward Gateway** side, you need to send the downloaded **Metadata XML** to [Reward Gateway support team](mailto:clientsupport@rewardgateway.com).</span></span> <span data-ttu-id="e8afd-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="e8afd-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e8afd-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e8afd-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e8afd-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e8afd-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e8afd-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e8afd-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e8afd-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e8afd-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="e8afd-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e8afd-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e8afd-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e8afd-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e8afd-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e8afd-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e8afd-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e8afd-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8afd-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e8afd-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e8afd-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e8afd-185">a.</span><span class="sxs-lookup"><span data-stu-id="e8afd-185">a.</span></span> <span data-ttu-id="e8afd-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e8afd-187">b.</span><span class="sxs-lookup"><span data-stu-id="e8afd-187">b.</span></span> <span data-ttu-id="e8afd-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e8afd-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e8afd-189">c.</span><span class="sxs-lookup"><span data-stu-id="e8afd-189">c.</span></span> <span data-ttu-id="e8afd-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e8afd-191">d.</span><span class="sxs-lookup"><span data-stu-id="e8afd-191">d.</span></span> <span data-ttu-id="e8afd-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-192">Click **Create**.</span></span>
 
### <a name="creating-a-reward-gateway-test-user"></a><span data-ttu-id="e8afd-193">Vytvoření zkušebního uživatele potřebu brány</span><span class="sxs-lookup"><span data-stu-id="e8afd-193">Creating a Reward Gateway test user</span></span>

<span data-ttu-id="e8afd-194">V této části vytvoříte uživatele volat Britta Simon potřebu brány.</span><span class="sxs-lookup"><span data-stu-id="e8afd-194">In this section, you create a user called Britta Simon in Reward Gateway.</span></span> <span data-ttu-id="e8afd-195">Práce s bránou potřebu [tým podpory](mailto:clientsupport@rewardgateway.com) v platformě potřebu brány přidat uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8afd-195">Work with Reward Gateway [support team](mailto:clientsupport@rewardgateway.com) to add the users in the Reward Gateway platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e8afd-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e8afd-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e8afd-197">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k bráně potřebu.</span><span class="sxs-lookup"><span data-stu-id="e8afd-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Reward Gateway.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e8afd-199">**Pokud chcete přiřadit Britta Simon potřebu brány, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="e8afd-199">**To assign Britta Simon to Reward Gateway, perform the following steps:**</span></span>

1. <span data-ttu-id="e8afd-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e8afd-202">V seznamu aplikací vyberte **potřebu brány**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-202">In the applications list, select **Reward Gateway**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_app.png) 

3. <span data-ttu-id="e8afd-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e8afd-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e8afd-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e8afd-206">Click **Add** button.</span></span> <span data-ttu-id="e8afd-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8afd-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e8afd-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e8afd-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e8afd-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8afd-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e8afd-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e8afd-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e8afd-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8afd-212">Testing single sign-on</span></span>

<span data-ttu-id="e8afd-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e8afd-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e8afd-214">Když kliknete na dlaždici potřebu brány na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci potřebu brány.</span><span class="sxs-lookup"><span data-stu-id="e8afd-214">When you click the Reward Gateway tile in the Access Panel, you should get automatically signed-on to your Reward Gateway application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8afd-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e8afd-215">Additional resources</span></span>

* [<span data-ttu-id="e8afd-216">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e8afd-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e8afd-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e8afd-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_203.png

