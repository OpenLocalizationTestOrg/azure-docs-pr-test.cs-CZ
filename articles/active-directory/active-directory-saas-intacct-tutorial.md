---
title: 'Kurz: Azure Active Directory integrace s Intacct | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Intacct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c203b192b9da0d280cbd7f6c123219242ee4a3d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a><span data-ttu-id="96f49-103">Kurz: Azure Active Directory integrace s Intacct</span><span class="sxs-lookup"><span data-stu-id="96f49-103">Tutorial: Azure Active Directory integration with Intacct</span></span>

<span data-ttu-id="96f49-104">V tomto kurzu zjistěte, jak integrovat Intacct s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="96f49-104">In this tutorial, you learn how to integrate Intacct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="96f49-105">Integrace Intacct s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="96f49-105">Integrating Intacct with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="96f49-106">Můžete řídit ve službě Azure AD, který má přístup k Intacct</span><span class="sxs-lookup"><span data-stu-id="96f49-106">You can control in Azure AD who has access to Intacct</span></span>
- <span data-ttu-id="96f49-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Intacct (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="96f49-107">You can enable your users to automatically get signed-on to Intacct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="96f49-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="96f49-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="96f49-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="96f49-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96f49-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96f49-110">Prerequisites</span></span>

<span data-ttu-id="96f49-111">Konfigurace integrace Azure AD s Intacct, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="96f49-111">To configure Azure AD integration with Intacct, you need the following items:</span></span>

- <span data-ttu-id="96f49-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="96f49-112">An Azure AD subscription</span></span>
- <span data-ttu-id="96f49-113">Intacct jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="96f49-113">An Intacct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="96f49-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="96f49-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="96f49-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="96f49-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="96f49-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="96f49-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="96f49-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="96f49-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="96f49-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="96f49-118">Scenario description</span></span>
<span data-ttu-id="96f49-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="96f49-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="96f49-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="96f49-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="96f49-121">Přidání Intacct z Galerie</span><span class="sxs-lookup"><span data-stu-id="96f49-121">Adding Intacct from the gallery</span></span>
2. <span data-ttu-id="96f49-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="96f49-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intacct-from-the-gallery"></a><span data-ttu-id="96f49-123">Přidání Intacct z Galerie</span><span class="sxs-lookup"><span data-stu-id="96f49-123">Adding Intacct from the gallery</span></span>
<span data-ttu-id="96f49-124">Při konfiguraci integrace Intacct do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Intacct z galerie.</span><span class="sxs-lookup"><span data-stu-id="96f49-124">To configure the integration of Intacct into Azure AD, you need to add Intacct from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="96f49-125">**Pokud chcete přidat Intacct z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="96f49-125">**To add Intacct from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="96f49-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="96f49-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="96f49-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="96f49-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="96f49-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="96f49-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="96f49-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="96f49-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="96f49-133">Do vyhledávacího pole zadejte **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="96f49-133">In the search box, type **Intacct**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. <span data-ttu-id="96f49-135">Na panelu výsledků vyberte **Intacct**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96f49-135">In the results panel, select **Intacct**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="96f49-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="96f49-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="96f49-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Intacct podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="96f49-138">In this section, you configure and test Azure AD single sign-on with Intacct based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="96f49-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Intacct je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="96f49-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Intacct is to a user in Azure AD.</span></span> <span data-ttu-id="96f49-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Intacct musí navázat.</span><span class="sxs-lookup"><span data-stu-id="96f49-140">In other words, a link relationship between an Azure AD user and the related user in Intacct needs to be established.</span></span>

<span data-ttu-id="96f49-141">V Intacct, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="96f49-141">In Intacct, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="96f49-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Intacct, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="96f49-142">To configure and test Azure AD single sign-on with Intacct, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="96f49-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="96f49-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="96f49-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="96f49-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="96f49-145">**[Vytváření testovacího uživatele Intacct](#creating-an-intacct-test-user)**  – Pokud chcete mít protějšek Britta Simon v Intacct propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="96f49-145">**[Creating an Intacct test user](#creating-an-intacct-test-user)** - to have a counterpart of Britta Simon in Intacct that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="96f49-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="96f49-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="96f49-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96f49-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="96f49-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="96f49-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="96f49-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Intacct.</span><span class="sxs-lookup"><span data-stu-id="96f49-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Intacct application.</span></span>

<span data-ttu-id="96f49-150">**Ke konfiguraci Azure AD jednotné přihlašování s Intacct, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="96f49-150">**To configure Azure AD single sign-on with Intacct, perform the following steps:**</span></span>

1. <span data-ttu-id="96f49-151">Na portálu Azure na **Intacct** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="96f49-151">In the Azure portal, on the **Intacct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="96f49-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="96f49-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. <span data-ttu-id="96f49-155">Na **Intacct domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="96f49-155">On the **Intacct Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    <span data-ttu-id="96f49-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="96f49-157">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > <span data-ttu-id="96f49-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="96f49-158">This value is not real.</span></span> <span data-ttu-id="96f49-159">Aktualizujte tuto hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="96f49-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="96f49-160">Obraťte se na [tým podpory Intacct](https://us.intacct.com/support) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96f49-160">Contact [Intacct support team](https://us.intacct.com/support) to get this value.</span></span>

4. <span data-ttu-id="96f49-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="96f49-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. <span data-ttu-id="96f49-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="96f49-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="96f49-165">Na **Intacct konfigurace** klikněte na tlačítko **konfigurace Intacct** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="96f49-165">On the **Intacct Configuration** section, click **Configure Intacct** to open **Configure sign-on** window.</span></span> <span data-ttu-id="96f49-166">Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="96f49-166">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. <span data-ttu-id="96f49-168">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Intacct jako správce.</span><span class="sxs-lookup"><span data-stu-id="96f49-168">In a different web browser window, sign in to your Intacct company site as an administrator.</span></span>

8. <span data-ttu-id="96f49-169">Klikněte **společnosti** a pak klikněte **informace společnosti**.</span><span class="sxs-lookup"><span data-stu-id="96f49-169">Click the **Company** tab, and then click **Company Info**.</span></span>

    <span data-ttu-id="96f49-170">![Společnosti](./media/active-directory-saas-intacct-tutorial/ic790037.png "společnosti")</span><span class="sxs-lookup"><span data-stu-id="96f49-170">![Company](./media/active-directory-saas-intacct-tutorial/ic790037.png "Company")</span></span>

9. <span data-ttu-id="96f49-171">Klikněte **zabezpečení** a pak klikněte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="96f49-171">Click the **Security** tab, and then click **Edit**.</span></span>

    <span data-ttu-id="96f49-172">![Zabezpečení](./media/active-directory-saas-intacct-tutorial/ic790038.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="96f49-172">![Security](./media/active-directory-saas-intacct-tutorial/ic790038.png "Security")</span></span>

10. <span data-ttu-id="96f49-173">V **jednotné přihlašování (SSO)** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="96f49-173">In the **Single sign on (SSO)** section, perform the following steps:</span></span>

    <span data-ttu-id="96f49-174">![Jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/ic790039.png "jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="96f49-174">![Single sign on](./media/active-directory-saas-intacct-tutorial/ic790039.png "single sign on")</span></span>

    <span data-ttu-id="96f49-175">a.</span><span class="sxs-lookup"><span data-stu-id="96f49-175">a.</span></span> <span data-ttu-id="96f49-176">Vyberte **povolení jednotného přihlašování na**.</span><span class="sxs-lookup"><span data-stu-id="96f49-176">Select **Enable single sign on**.</span></span>

    <span data-ttu-id="96f49-177">b.</span><span class="sxs-lookup"><span data-stu-id="96f49-177">b.</span></span> <span data-ttu-id="96f49-178">Jako **typ zprostředkovatele Identity**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="96f49-178">As **Identity provider type**, select **SAML 2.0**.</span></span>

    <span data-ttu-id="96f49-179">c.</span><span class="sxs-lookup"><span data-stu-id="96f49-179">c.</span></span> <span data-ttu-id="96f49-180">V **URL vystavitele** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="96f49-180">In **Issuer URL** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="96f49-181">d.</span><span class="sxs-lookup"><span data-stu-id="96f49-181">d.</span></span> <span data-ttu-id="96f49-182">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="96f49-182">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="96f49-183">e.</span><span class="sxs-lookup"><span data-stu-id="96f49-183">e.</span></span> <span data-ttu-id="96f49-184">Otevřete váš **kódování base-64** kódování certifikátu v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="96f49-184">Open your **base-64** encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** box.</span></span>
   
    <span data-ttu-id="96f49-185">f.</span><span class="sxs-lookup"><span data-stu-id="96f49-185">f.</span></span> <span data-ttu-id="96f49-186">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="96f49-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="96f49-187">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="96f49-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="96f49-188">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="96f49-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="96f49-189">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="96f49-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="96f49-190">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="96f49-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="96f49-191">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="96f49-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="96f49-193">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="96f49-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="96f49-194">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="96f49-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="96f49-196">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="96f49-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="96f49-198">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="96f49-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="96f49-200">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="96f49-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="96f49-202">a.</span><span class="sxs-lookup"><span data-stu-id="96f49-202">a.</span></span> <span data-ttu-id="96f49-203">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="96f49-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="96f49-204">b.</span><span class="sxs-lookup"><span data-stu-id="96f49-204">b.</span></span> <span data-ttu-id="96f49-205">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="96f49-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="96f49-206">c.</span><span class="sxs-lookup"><span data-stu-id="96f49-206">c.</span></span> <span data-ttu-id="96f49-207">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="96f49-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="96f49-208">d.</span><span class="sxs-lookup"><span data-stu-id="96f49-208">d.</span></span> <span data-ttu-id="96f49-209">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="96f49-209">Click **Create**.</span></span>
 
### <a name="creating-an-intacct-test-user"></a><span data-ttu-id="96f49-210">Vytváření testovacího uživatele Intacct</span><span class="sxs-lookup"><span data-stu-id="96f49-210">Creating an Intacct test user</span></span>

<span data-ttu-id="96f49-211">Nastavit uživatele Azure AD, takže se můžete přihlásit k Intacct, musí být zřízená do Intacct.</span><span class="sxs-lookup"><span data-stu-id="96f49-211">To set up Azure AD users so they can sign in to Intacct, they must be provisioned into Intacct.</span></span> <span data-ttu-id="96f49-212">Pro Intacct zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="96f49-212">For Intacct, provisioning is a manual task.</span></span>

<span data-ttu-id="96f49-213">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="96f49-213">**To provision user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="96f49-214">Přihlaste se k vaší **Intacct** klienta.</span><span class="sxs-lookup"><span data-stu-id="96f49-214">Sign in to your **Intacct** tenant.</span></span>

2. <span data-ttu-id="96f49-215">Klikněte **společnosti** a pak klikněte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="96f49-215">Click the **Company** tab, and then click **Users**.</span></span>

    <span data-ttu-id="96f49-216">![Uživatelé](./media/active-directory-saas-intacct-tutorial/ic790041.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="96f49-216">![Users](./media/active-directory-saas-intacct-tutorial/ic790041.png "Users")</span></span>
3. <span data-ttu-id="96f49-217">Klikněte **přidat** kartě.</span><span class="sxs-lookup"><span data-stu-id="96f49-217">Click the **Add** tab.</span></span>

    <span data-ttu-id="96f49-218">![Přidat](./media/active-directory-saas-intacct-tutorial/ic790042.png "přidat")</span><span class="sxs-lookup"><span data-stu-id="96f49-218">![Add](./media/active-directory-saas-intacct-tutorial/ic790042.png "Add")</span></span>
4. <span data-ttu-id="96f49-219">V **informace o uživateli** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="96f49-219">In the **User Information** section, perform the following steps:</span></span>

    <span data-ttu-id="96f49-220">![Informace o uživateli](./media/active-directory-saas-intacct-tutorial/ic790043.png "informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="96f49-220">![User Information](./media/active-directory-saas-intacct-tutorial/ic790043.png "User Information")</span></span>

    <span data-ttu-id="96f49-221">a.</span><span class="sxs-lookup"><span data-stu-id="96f49-221">a.</span></span> <span data-ttu-id="96f49-222">Zadejte **ID uživatele**, **příjmení**, **křestní jméno**, **e-mailová adresa**, **název**a **Phone** účtu Azure AD, který chcete mají být zahrnuty do **informace o uživateli** části.</span><span class="sxs-lookup"><span data-stu-id="96f49-222">Enter the **User ID**, the **Last name**, **First name**, the **Email address**, the **Title**, and the **Phone** of an Azure AD account that you want to provision into the **User Information** section.</span></span>

    <span data-ttu-id="96f49-223">b.</span><span class="sxs-lookup"><span data-stu-id="96f49-223">b.</span></span> <span data-ttu-id="96f49-224">Vyberte **oprávnění správce** účtu Azure AD, který chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="96f49-224">Select the **Admin privileges** of an Azure AD account that you want to provision.</span></span>
   
    <span data-ttu-id="96f49-225">c.</span><span class="sxs-lookup"><span data-stu-id="96f49-225">c.</span></span> <span data-ttu-id="96f49-226">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="96f49-226">Click **Save**.</span></span> <span data-ttu-id="96f49-227">Držitel účtu Azure AD obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="96f49-227">The Azure AD account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

>[!NOTE]
><span data-ttu-id="96f49-228">Ke zřízení uživatelských účtů Azure AD, můžete použít jiné nástroje pro tvorbu Intacct uživatelského účtu nebo rozhraní API, které jsou poskytovány Intacct.</span><span class="sxs-lookup"><span data-stu-id="96f49-228">To provision Azure AD user accounts, you can use other Intacct user account creation tools or APIs that are provided by Intacct.</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="96f49-229">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="96f49-229">Assigning the Azure AD test user</span></span>

<span data-ttu-id="96f49-230">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Intacct.</span><span class="sxs-lookup"><span data-stu-id="96f49-230">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Intacct.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="96f49-232">**Pokud chcete přiřadit Britta Simon Intacct, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="96f49-232">**To assign Britta Simon to Intacct, perform the following steps:**</span></span>

1. <span data-ttu-id="96f49-233">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="96f49-233">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="96f49-235">V seznamu aplikací vyberte **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="96f49-235">In the applications list, select **Intacct**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. <span data-ttu-id="96f49-237">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="96f49-237">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="96f49-239">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="96f49-239">Click **Add** button.</span></span> <span data-ttu-id="96f49-240">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="96f49-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="96f49-242">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="96f49-242">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="96f49-243">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="96f49-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="96f49-244">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="96f49-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="96f49-245">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="96f49-245">Testing single sign-on</span></span>

<span data-ttu-id="96f49-246">V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="96f49-246">In this section, you test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="96f49-247">Když kliknete na dlaždici Intacct na přístupovém panelu, můžete by měl být automaticky přihlášeni do vaší aplikace Intacct.</span><span class="sxs-lookup"><span data-stu-id="96f49-247">When you click the Intacct tile in the Access Panel, you should be automatically signed in to your Intacct application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96f49-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="96f49-248">Additional resources</span></span>

* [<span data-ttu-id="96f49-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="96f49-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="96f49-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="96f49-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

