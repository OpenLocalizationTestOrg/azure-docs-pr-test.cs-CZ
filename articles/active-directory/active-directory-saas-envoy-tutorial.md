---
title: 'Kurz: Azure Active Directory integrace s Envoy | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Envoy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71f7afcc-1033-4098-9b7e-4f9f2b26f734
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 49211b35ab3e28e0df914061e7fa623907935638
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-envoy"></a><span data-ttu-id="60075-103">Kurz: Azure Active Directory integrace s Envoy</span><span class="sxs-lookup"><span data-stu-id="60075-103">Tutorial: Azure Active Directory integration with Envoy</span></span>

<span data-ttu-id="60075-104">V tomto kurzu zjistěte, jak integrovat Envoy s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="60075-104">In this tutorial, you learn how to integrate Envoy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="60075-105">Integrace Envoy s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="60075-105">Integrating Envoy with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="60075-106">Můžete ovládat ve službě Azure AD, který má přístup k Envoy.</span><span class="sxs-lookup"><span data-stu-id="60075-106">You can control in Azure AD who has access to Envoy.</span></span>
- <span data-ttu-id="60075-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Envoy (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="60075-107">You can enable your users to automatically get signed-on to Envoy (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="60075-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="60075-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="60075-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="60075-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60075-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60075-110">Prerequisites</span></span>

<span data-ttu-id="60075-111">Konfigurace integrace Azure AD s Envoy, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="60075-111">To configure Azure AD integration with Envoy, you need the following items:</span></span>

- <span data-ttu-id="60075-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="60075-112">An Azure AD subscription</span></span>
- <span data-ttu-id="60075-113">Envoy jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="60075-113">An Envoy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="60075-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="60075-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="60075-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="60075-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="60075-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="60075-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="60075-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="60075-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="60075-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="60075-118">Scenario description</span></span>
<span data-ttu-id="60075-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="60075-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="60075-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="60075-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="60075-121">Přidání Envoy z Galerie</span><span class="sxs-lookup"><span data-stu-id="60075-121">Adding Envoy from the gallery</span></span>
2. <span data-ttu-id="60075-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="60075-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-envoy-from-the-gallery"></a><span data-ttu-id="60075-123">Přidání Envoy z Galerie</span><span class="sxs-lookup"><span data-stu-id="60075-123">Adding Envoy from the gallery</span></span>
<span data-ttu-id="60075-124">Při konfiguraci integrace Envoy do služby Azure AD potřebujete přidat Envoy z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="60075-124">To configure the integration of Envoy into Azure AD, you need to add Envoy from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="60075-125">**Pokud chcete přidat Envoy z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60075-125">**To add Envoy from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="60075-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="60075-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="60075-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="60075-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="60075-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="60075-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="60075-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60075-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="60075-133">Do vyhledávacího pole zadejte **Envoy**, vyberte **Envoy** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="60075-133">In the search box, type **Envoy**, select **Envoy** from result panel then click **Add** button to add the application.</span></span>

    ![Envoy v seznamu výsledků](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="60075-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="60075-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="60075-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Envoy podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="60075-136">In this section, you configure and test Azure AD single sign-on with Envoy based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="60075-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Envoy je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="60075-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Envoy is to a user in Azure AD.</span></span> <span data-ttu-id="60075-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Envoy musí navázat.</span><span class="sxs-lookup"><span data-stu-id="60075-138">In other words, a link relationship between an Azure AD user and the related user in Envoy needs to be established.</span></span>

<span data-ttu-id="60075-139">V Envoy, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="60075-139">In Envoy, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="60075-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Envoy, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="60075-140">To configure and test Azure AD single sign-on with Envoy, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="60075-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="60075-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="60075-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60075-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="60075-143">**[Vytvořit testovací uživatele s Envoy](#create-an-envoy-test-user)**  – Pokud chcete mít protějšek Britta Simon v Envoy propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="60075-143">**[Create an Envoy test user](#create-an-envoy-test-user)** - to have a counterpart of Britta Simon in Envoy that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="60075-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60075-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="60075-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="60075-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="60075-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="60075-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="60075-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Envoy.</span><span class="sxs-lookup"><span data-stu-id="60075-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Envoy application.</span></span>

<span data-ttu-id="60075-148">**Ke konfiguraci Azure AD jednotné přihlašování s Envoy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60075-148">**To configure Azure AD single sign-on with Envoy, perform the following steps:**</span></span>

1. <span data-ttu-id="60075-149">Na portálu Azure na **Envoy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="60075-149">In the Azure portal, on the **Envoy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="60075-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60075-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_samlbase.png)

3. <span data-ttu-id="60075-153">Na **Envoy domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60075-153">On the **Envoy Domain and URLs** section, perform the following steps:</span></span>

    ![Envoy domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_url.png)

    <span data-ttu-id="60075-155">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.Envoy.com`</span><span class="sxs-lookup"><span data-stu-id="60075-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.Envoy.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="60075-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="60075-156">This value is not real.</span></span> <span data-ttu-id="60075-157">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60075-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="60075-158">Obraťte se na [tým podpory Envoy klienta](https://envoy.com/contact/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="60075-158">Contact [Envoy Client support team](https://envoy.com/contact/) to get this value.</span></span>

4. <span data-ttu-id="60075-159">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu...</span><span class="sxs-lookup"><span data-stu-id="60075-159">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate..</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_certificate.png) 

5. <span data-ttu-id="60075-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60075-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-envoy-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="60075-163">Na **Envoy konfigurace** klikněte na tlačítko **konfigurace Envoy** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="60075-163">On the **Envoy Configuration** section, click **Configure Envoy** to open **Configure sign-on** window.</span></span> <span data-ttu-id="60075-164">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="60075-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Envoy](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_configure.png)

7. <span data-ttu-id="60075-166">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Envoy.</span><span class="sxs-lookup"><span data-stu-id="60075-166">In a different web browser window, log into your Envoy company site as an administrator.</span></span>

8. <span data-ttu-id="60075-167">Na panelu nástrojů v horní části klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="60075-167">In the toolbar on the top, click **Settings**.</span></span>

    <span data-ttu-id="60075-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span><span class="sxs-lookup"><span data-stu-id="60075-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span></span>

9. <span data-ttu-id="60075-169">Klikněte na tlačítko **společnosti**.</span><span class="sxs-lookup"><span data-stu-id="60075-169">Click **Company**.</span></span>

    <span data-ttu-id="60075-170">![Společnosti](./media/active-directory-saas-envoy-tutorial/ic776783.png "společnosti")</span><span class="sxs-lookup"><span data-stu-id="60075-170">![Company](./media/active-directory-saas-envoy-tutorial/ic776783.png "Company")</span></span>

10. <span data-ttu-id="60075-171">Klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="60075-171">Click **SAML**.</span></span>

    <span data-ttu-id="60075-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="60075-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span></span>

11. <span data-ttu-id="60075-173">V **ověřování SAML** konfigurace části, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60075-173">In the **SAML Authentication** configuration section, perform the following steps:</span></span>

    <span data-ttu-id="60075-174">![Ověřování SAML](./media/active-directory-saas-envoy-tutorial/ic776785.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="60075-174">![SAML authentication](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML authentication")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="60075-175">Hodnota pro Identifikátor umístění Ústředí je automaticky generovaný aplikací.</span><span class="sxs-lookup"><span data-stu-id="60075-175">The value for the HQ location ID is auto generated by the application.</span></span>
    
    <span data-ttu-id="60075-176">a.</span><span class="sxs-lookup"><span data-stu-id="60075-176">a.</span></span> <span data-ttu-id="60075-177">V **otisk prstu** textovému poli, Vložit **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="60075-177">In **Fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="60075-178">b.</span><span class="sxs-lookup"><span data-stu-id="60075-178">b.</span></span> <span data-ttu-id="60075-179">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali formuláři na portálu Azure do **adresa URL IDENTITY zprostředkovatele protokolu HTTP SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="60075-179">Paste **SAML Single Sign-On Service URL** value, which you have copied form the Azure portal into the **IDENTITY PROVIDER HTTP SAML URL** textbox.</span></span>
    
    <span data-ttu-id="60075-180">c.</span><span class="sxs-lookup"><span data-stu-id="60075-180">c.</span></span> <span data-ttu-id="60075-181">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="60075-181">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="60075-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="60075-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="60075-183">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="60075-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="60075-184">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="60075-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="60075-185">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="60075-185">Create an Azure AD test user</span></span>

<span data-ttu-id="60075-186">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60075-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="60075-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60075-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="60075-189">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60075-189">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-envoy-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="60075-191">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="60075-191">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-envoy-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="60075-193">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60075-193">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-envoy-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="60075-195">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60075-195">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-envoy-tutorial/create_aaduser_04.png)

    <span data-ttu-id="60075-197">a.</span><span class="sxs-lookup"><span data-stu-id="60075-197">a.</span></span> <span data-ttu-id="60075-198">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="60075-198">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="60075-199">b.</span><span class="sxs-lookup"><span data-stu-id="60075-199">b.</span></span> <span data-ttu-id="60075-200">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60075-200">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="60075-201">c.</span><span class="sxs-lookup"><span data-stu-id="60075-201">c.</span></span> <span data-ttu-id="60075-202">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="60075-202">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="60075-203">d.</span><span class="sxs-lookup"><span data-stu-id="60075-203">d.</span></span> <span data-ttu-id="60075-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="60075-204">Click **Create**.</span></span>
 
### <a name="create-an-envoy-test-user"></a><span data-ttu-id="60075-205">Vytvořit uživatele s Envoy testu</span><span class="sxs-lookup"><span data-stu-id="60075-205">Create an Envoy test user</span></span>

<span data-ttu-id="60075-206">Neexistuje žádná položka akce můžete nakonfigurovat na Envoy zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="60075-206">There is no action item for you to configure user provisioning to Envoy.</span></span> <span data-ttu-id="60075-207">Když přiřazený uživatel se pokusí přihlásit pomocí přístupového panelu Envoy, Envoy ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="60075-207">When an assigned user tries to log into Envoy using the access panel, Envoy checks whether the user exists.</span></span> <span data-ttu-id="60075-208">Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří pomocí Envoy.</span><span class="sxs-lookup"><span data-stu-id="60075-208">If there is no user account available yet, it is automatically created by Envoy.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="60075-209">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="60075-209">Assign the Azure AD test user</span></span>

<span data-ttu-id="60075-210">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Envoy.</span><span class="sxs-lookup"><span data-stu-id="60075-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Envoy.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="60075-212">**Pokud chcete přiřadit Britta Simon Envoy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60075-212">**To assign Britta Simon to Envoy, perform the following steps:**</span></span>

1. <span data-ttu-id="60075-213">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="60075-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="60075-215">V seznamu aplikací vyberte **Envoy**.</span><span class="sxs-lookup"><span data-stu-id="60075-215">In the applications list, select **Envoy**.</span></span>

    ![V seznamu aplikací na Envoy odkaz](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_app.png)  

3. <span data-ttu-id="60075-217">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="60075-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="60075-219">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60075-219">Click **Add** button.</span></span> <span data-ttu-id="60075-220">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60075-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="60075-222">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="60075-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="60075-223">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60075-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="60075-224">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60075-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="60075-225">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="60075-225">Test single sign-on</span></span>

<span data-ttu-id="60075-226">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="60075-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="60075-227">Když kliknete na dlaždici Envoy na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Envoy.</span><span class="sxs-lookup"><span data-stu-id="60075-227">When you click the Envoy tile in the Access Panel, you should get automatically signed-on to your Envoy application.</span></span>
<span data-ttu-id="60075-228">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="60075-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="60075-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="60075-229">Additional resources</span></span>

* [<span data-ttu-id="60075-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="60075-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="60075-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="60075-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_203.png

