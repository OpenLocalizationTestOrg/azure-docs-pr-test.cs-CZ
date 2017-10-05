---
title: 'Kurz: Azure Active Directory integrace s Clarizen | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Clarizen."
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
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 574c6877bddac8be7d6d541bfabbdc10f6be3101
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a><span data-ttu-id="066f3-103">Kurz: Azure Active Directory integrace s Clarizen</span><span class="sxs-lookup"><span data-stu-id="066f3-103">Tutorial: Azure Active Directory integration with Clarizen</span></span>

<span data-ttu-id="066f3-104">V tomto kurzu jste zjistěte, jak integrovat Azure Active Directory (Azure AD) s Clarizen.</span><span class="sxs-lookup"><span data-stu-id="066f3-104">In this tutorial, you learn how to integrate Azure Active Directory (Azure AD) with Clarizen.</span></span> <span data-ttu-id="066f3-105">Tato integrace poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="066f3-105">This integration gives you the following benefits:</span></span>

- <span data-ttu-id="066f3-106">Můžete ovládat, ve službě Azure AD, který má přístup k Clarizen.</span><span class="sxs-lookup"><span data-stu-id="066f3-106">You can control, in Azure AD, who has access to Clarizen.</span></span>
- <span data-ttu-id="066f3-107">Můžete povolit uživatelům, aby se automaticky přihlásíte k Clarizen (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="066f3-107">You can enable your users to be automatically signed in to Clarizen (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="066f3-108">Můžete spravovat vaše účty v jednom centrálním místě, portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="066f3-108">You can manage your accounts in one central location, the Azure portal.</span></span>

<span data-ttu-id="066f3-109">Tento scénář v tomto kurzu se skládá z dvě hlavní úlohy:</span><span class="sxs-lookup"><span data-stu-id="066f3-109">The scenario in this tutorial consists of two main tasks:</span></span>

1. <span data-ttu-id="066f3-110">Přidejte Clarizen z galerie.</span><span class="sxs-lookup"><span data-stu-id="066f3-110">Add Clarizen from the gallery.</span></span>
2. <span data-ttu-id="066f3-111">Konfigurace a otestování Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="066f3-111">Configure and test Azure AD single sign-on.</span></span>

<span data-ttu-id="066f3-112">Pokud chcete další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="066f3-112">If you want more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="066f3-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="066f3-113">Prerequisites</span></span>
<span data-ttu-id="066f3-114">Konfigurace integrace Azure AD s Clarizen, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="066f3-114">To configure Azure AD integration with Clarizen, you need the following items:</span></span>

- <span data-ttu-id="066f3-115">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="066f3-115">An Azure AD subscription</span></span>
- <span data-ttu-id="066f3-116">Clarizen odběr, který je povolený pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="066f3-116">A Clarizen subscription that's enabled for single sign-on</span></span>

<span data-ttu-id="066f3-117">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="066f3-117">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="066f3-118">Testování Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="066f3-118">Test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="066f3-119">Nepoužívejte produkční prostředí, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="066f3-119">Don't use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="066f3-120">Pokud nemáte testovacím prostředí s Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="066f3-120">If you don't have an Azure AD test environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="add-clarizen-from-the-gallery"></a><span data-ttu-id="066f3-121">Přidat Clarizen z Galerie</span><span class="sxs-lookup"><span data-stu-id="066f3-121">Add Clarizen from the gallery</span></span>
<span data-ttu-id="066f3-122">Při konfiguraci integrace Clarizen do služby Azure AD přidáte do seznamu spravovaných aplikací SaaS Clarizen z galerie.</span><span class="sxs-lookup"><span data-stu-id="066f3-122">To configure the integration of Clarizen into Azure AD, add Clarizen from the gallery to your list of managed SaaS apps.</span></span>

1. <span data-ttu-id="066f3-123">V [portál Azure](https://portal.azure.com), v levém podokně klikněte **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="066f3-123">In the [Azure portal](https://portal.azure.com), in the left pane, click the **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory – ikona][1]

2. <span data-ttu-id="066f3-125">Klikněte na tlačítko **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="066f3-125">Click **Enterprise applications**.</span></span> <span data-ttu-id="066f3-126">Pak klikněte na tlačítko **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="066f3-126">Then click **All applications**.</span></span>

    ![Kliknutím na tlačítko "Podnikové aplikace" a "Všechny aplikace"][2]

3. <span data-ttu-id="066f3-128">Klikněte **přidat** tlačítka v horní části dialogových oken.</span><span class="sxs-lookup"><span data-stu-id="066f3-128">Click the **Add** button at the top of the dialog box.</span></span>

    ![Tlačítko "Přidat"][3]

4. <span data-ttu-id="066f3-130">Do vyhledávacího pole zadejte **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="066f3-130">In the search box, type **Clarizen**.</span></span>

    ![Do vyhledávacího pole zadejte "Clarizen"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. <span data-ttu-id="066f3-132">V podokně výsledků vyberte **Clarizen**a potom klikněte na **přidat** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="066f3-132">In the results pane, select **Clarizen**, and then click **Add** to add the application.</span></span>

    ![Výběr Clarizen v podokně výsledků](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="066f3-134">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="066f3-134">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="066f3-135">V následujících částech nakonfigurovat a otestovat Azure AD jednotné přihlašování s Clarizen na základě testovací uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="066f3-135">In the following sections, you configure and test Azure AD single sign-on with Clarizen based on the test user Britta Simon.</span></span>

<span data-ttu-id="066f3-136">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Clarizen je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="066f3-136">For single sign-on to work, Azure AD needs to know what the counterpart user in Clarizen is to a user in Azure AD.</span></span> <span data-ttu-id="066f3-137">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Clarizen musí navázat.</span><span class="sxs-lookup"><span data-stu-id="066f3-137">In other words, a link relationship between an Azure AD user and the related user in Clarizen needs to be established.</span></span> <span data-ttu-id="066f3-138">Vytvořit tento odkaz vztah přiřazením hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Clarizen.</span><span class="sxs-lookup"><span data-stu-id="066f3-138">You establish this link relationship by assigning the value of **user name** in Azure AD as the value of **Username** in Clarizen.</span></span>

<span data-ttu-id="066f3-139">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Clarizen, proveďte následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="066f3-139">To configure and test Azure AD single sign-on with Clarizen, complete the following building blocks:</span></span>

1. <span data-ttu-id="066f3-140">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  umožňující uživatelům používat tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="066f3-140">**[Configure Azure AD single sign-on](#configure-azure-ad-single-sign-on)** to enable your users to use this feature.</span></span>
2. <span data-ttu-id="066f3-141">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  k testování Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="066f3-141">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="066f3-142">**[Vytvoření zkušebního uživatele Clarizen](#create-a-clarizen-test-user)**  tak, aby měl protějšek Britta Simon v Clarizen propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="066f3-142">**[Create a Clarizen test user](#create-a-clarizen-test-user)** to have a counterpart of Britta Simon in Clarizen that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="066f3-143">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="066f3-143">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="066f3-144">**[Test jednotného přihlašování](#test-single-sign-on)**  ověřit, zda funguje konfigurace.</span><span class="sxs-lookup"><span data-stu-id="066f3-144">**[Test single sign-on](#test-single-sign-on)** to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="066f3-145">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="066f3-145">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="066f3-146">Povolení služby Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Clarizen.</span><span class="sxs-lookup"><span data-stu-id="066f3-146">Enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Clarizen application.</span></span>

1. <span data-ttu-id="066f3-147">Na portálu Azure na **Clarizen** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="066f3-147">In the Azure portal, on the **Clarizen** application integration page, click **Single sign-on**.</span></span>

    ![Kliknutím na tlačítko "Single sign-on"][4]

2. <span data-ttu-id="066f3-149">V **jednotného přihlašování** dialogové okno, pro **režimu**, vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="066f3-149">In the **Single sign-on** dialog box, for **Mode**, select **SAML-based Sign-on** to enable single sign-on.</span></span>

    ![Výběr "na základě SAML přihlášení"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. <span data-ttu-id="066f3-151">V **Clarizen domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="066f3-151">In the **Clarizen Domain and URLs** section, perform the following steps:</span></span>

    ![Pole pro adresu URL identifikátor dotazů a odpovědí](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    <span data-ttu-id="066f3-153">a.</span><span class="sxs-lookup"><span data-stu-id="066f3-153">a.</span></span> <span data-ttu-id="066f3-154">V **identifikátor** zadejte hodnotu jako: **Clarizen**</span><span class="sxs-lookup"><span data-stu-id="066f3-154">In the **Identifier** box, type the value as: **Clarizen**</span></span>

    <span data-ttu-id="066f3-155">b.</span><span class="sxs-lookup"><span data-stu-id="066f3-155">b.</span></span> <span data-ttu-id="066f3-156">V **adresa URL odpovědi** zadejte adresu URL pomocí následujícího vzorce: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span><span class="sxs-lookup"><span data-stu-id="066f3-156">In the **Reply URL** box, type a URL by using the following pattern: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span></span>

    > [!NOTE]
    > <span data-ttu-id="066f3-157">Tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="066f3-157">These are not the real values.</span></span> <span data-ttu-id="066f3-158">Budete muset použít identifikátor skutečné a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="066f3-158">You have to use the actual identifier and reply URL.</span></span> <span data-ttu-id="066f3-159">Tady doporučujeme použít jedinečnou hodnotu řetězce jako identifikátor.</span><span class="sxs-lookup"><span data-stu-id="066f3-159">Here we suggest that you use the unique value of a string as the identifier.</span></span> <span data-ttu-id="066f3-160">Chcete-li získat skutečnými hodnotami, obraťte se [tým podpory Clarizen](https://success.clarizen.com/hc/en-us/requests/new).</span><span class="sxs-lookup"><span data-stu-id="066f3-160">To get the actual values, contact the [Clarizen support team](https://success.clarizen.com/hc/en-us/requests/new).</span></span>

4. <span data-ttu-id="066f3-161">Na **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="066f3-161">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Kliknutím na tlačítko "Vytvořit nový certifikát"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. <span data-ttu-id="066f3-163">V **vytvořit nový certifikát** dialogové okno pole, klikněte na ikonu kalendáři a vyberte datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="066f3-163">In the **Create New Certificate** dialog box, click the calendar icon and select an expiry date.</span></span> <span data-ttu-id="066f3-164">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="066f3-164">Then click **Save**.</span></span>

    ![Výběr a ukládání datum vypršení platnosti](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="066f3-166">V **SAML podpisový certifikát** vyberte **aktivujte nový certifikát**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="066f3-166">In the **SAML Signing Certificate** section, select **Make new certificate active**, and then click **Save**.</span></span>

    ![Zaškrtněte políčko pro vytvoření nového certifikátu active](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. <span data-ttu-id="066f3-168">V **certifikát výměny** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="066f3-168">In the **Rollover certificate** dialog box, click **OK**.</span></span>

    ![Kliknutím na tlačítko "OK" potvrďte, že chcete aktivovat certifikát](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="066f3-170">V **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="066f3-170">In the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Kliknutím na tlačítko "Certifikát (Base64)" zahájíte stahování](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. <span data-ttu-id="066f3-172">V **Clarizen konfigurace** klikněte na tlačítko **konfigurace Clarizen** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="066f3-172">In the **Clarizen Configuration** section, click **Configure Clarizen** to open the **Configure sign-on** window.</span></span>

    ![Kliknutím na tlačítko "Konfigurace Clarizen"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    ![Okno "Konfigurace přihlášení", včetně souborů a adresy URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. <span data-ttu-id="066f3-175">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Clarizen jako správce.</span><span class="sxs-lookup"><span data-stu-id="066f3-175">In a different web browser window, sign in to your Clarizen company site as an administrator.</span></span>

11. <span data-ttu-id="066f3-176">Klikněte na své uživatelské jméno a potom klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="066f3-176">Click your username, and then click **Settings**.</span></span>

    <span data-ttu-id="066f3-177">![Kliknutím na "Nastavení" v části vaše uživatelské jméno](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="066f3-177">![Clicking "Settings" under your username](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "Settings")</span></span>

12. <span data-ttu-id="066f3-178">Klikněte **globální nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="066f3-178">Click the **Global Settings** tab.</span></span> <span data-ttu-id="066f3-179">Pak do **federovaného ověřování**, klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="066f3-179">Then, next to **Federated Authentication**, click **edit**.</span></span>

    <span data-ttu-id="066f3-180">![Karta "Globální nastavení"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globální nastavení")</span><span class="sxs-lookup"><span data-stu-id="066f3-180">!["Global Settings" tab](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "Global Settings")</span></span>

13. <span data-ttu-id="066f3-181">V **federovaného ověřování** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="066f3-181">In the **Federated Authentication** dialog box, perform the following steps:</span></span>

    <span data-ttu-id="066f3-182">![Dialogové okno "Federovaného ověřování"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "federovaného ověřování")</span><span class="sxs-lookup"><span data-stu-id="066f3-182">!["Federated Authentication" dialog box](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")</span></span>

    <span data-ttu-id="066f3-183">a.</span><span class="sxs-lookup"><span data-stu-id="066f3-183">a.</span></span> <span data-ttu-id="066f3-184">Vyberte **povolit federovaného ověřování**.</span><span class="sxs-lookup"><span data-stu-id="066f3-184">Select **Enable Federated Authentication**.</span></span>

    <span data-ttu-id="066f3-185">b.</span><span class="sxs-lookup"><span data-stu-id="066f3-185">b.</span></span> <span data-ttu-id="066f3-186">Klikněte na tlačítko **nahrát** nahrát stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="066f3-186">Click **Upload** to upload your downloaded certificate.</span></span>

    <span data-ttu-id="066f3-187">c.</span><span class="sxs-lookup"><span data-stu-id="066f3-187">c.</span></span> <span data-ttu-id="066f3-188">V **přihlašovací adresa URL** zadejte hodnotu **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="066f3-188">In the **Sign-in URL** box, enter the value of **SAML Single Sign-On Service URL** from the Azure AD application configuration window.</span></span>

    <span data-ttu-id="066f3-189">d.</span><span class="sxs-lookup"><span data-stu-id="066f3-189">d.</span></span> <span data-ttu-id="066f3-190">V **Sign-out URL** zadejte hodnotu **Sign-Out URL** z okna konfigurace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="066f3-190">In the **Sign-out URL** box, enter the value of **Sign-Out URL** from the Azure AD application configuration window.</span></span>

    <span data-ttu-id="066f3-191">e.</span><span class="sxs-lookup"><span data-stu-id="066f3-191">e.</span></span> <span data-ttu-id="066f3-192">Vyberte **použít POST**.</span><span class="sxs-lookup"><span data-stu-id="066f3-192">Select **Use POST**.</span></span>

    <span data-ttu-id="066f3-193">f.</span><span class="sxs-lookup"><span data-stu-id="066f3-193">f.</span></span> <span data-ttu-id="066f3-194">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="066f3-194">Click **Save**.</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="066f3-195">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="066f3-195">Create an Azure AD test user</span></span>
<span data-ttu-id="066f3-196">Na portálu Azure vytvořte testovací uživatele volat Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="066f3-196">In the Azure portal, create a test user called Britta Simon.</span></span>

![Jméno a e-mailovou adresu testovacího uživatele Azure AD][100]

1. <span data-ttu-id="066f3-198">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="066f3-198">In the Azure portal, in the left pane, click the **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory – ikona](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="066f3-200">Klikněte na tlačítko **uživatelů a skupin**a potom klikněte na **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="066f3-200">Click **Users and groups**, and then click **All users** to display the list of users.</span></span>

    ![Kliknutím na tlačítko "Uživatelé a skupiny" a "Všichni uživatelé"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="066f3-202">V horní části dialogových oken, klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="066f3-202">At the top of the dialog box, click **Add** to open the **User** dialog box.</span></span>

    ![Tlačítko "Přidat"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="066f3-204">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="066f3-204">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno "User" s název, e-mailovou adresu a heslo vyplněno](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    <span data-ttu-id="066f3-206">a.</span><span class="sxs-lookup"><span data-stu-id="066f3-206">a.</span></span> <span data-ttu-id="066f3-207">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="066f3-207">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="066f3-208">b.</span><span class="sxs-lookup"><span data-stu-id="066f3-208">b.</span></span> <span data-ttu-id="066f3-209">V **uživatelské jméno** zadejte e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="066f3-209">In the **User name** box, type the email address of the Britta Simon account.</span></span>

    <span data-ttu-id="066f3-210">c.</span><span class="sxs-lookup"><span data-stu-id="066f3-210">c.</span></span> <span data-ttu-id="066f3-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="066f3-211">Select **Show Password** and write down the value of **Password**.</span></span>

    <span data-ttu-id="066f3-212">d.</span><span class="sxs-lookup"><span data-stu-id="066f3-212">d.</span></span> <span data-ttu-id="066f3-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="066f3-213">Click **Create**.</span></span>

### <a name="create-a-clarizen-test-user"></a><span data-ttu-id="066f3-214">Vytvoření zkušebního uživatele Clarizen</span><span class="sxs-lookup"><span data-stu-id="066f3-214">Create a Clarizen test user</span></span>
<span data-ttu-id="066f3-215">Pokud chcete povolit uživatelům Azure AD přihlášení k Clarizen, je třeba zřídit uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="066f3-215">To enable Azure AD users to sign in to Clarizen, you must provision user accounts.</span></span> <span data-ttu-id="066f3-216">V případě Clarizen zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="066f3-216">In the case of Clarizen, provisioning is a manual task.</span></span>

1. <span data-ttu-id="066f3-217">Přihlaste se k serveru vaší společnosti Clarizen jako správce.</span><span class="sxs-lookup"><span data-stu-id="066f3-217">Sign in to your Clarizen company site as an administrator.</span></span>

2. <span data-ttu-id="066f3-218">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="066f3-218">Click **People**.</span></span>

    <span data-ttu-id="066f3-219">![Kliknutím na tlačítko "Uživatelé"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="066f3-219">![Clicking "People"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="066f3-220">Klikněte na tlačítko **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="066f3-220">Click **Invite User**.</span></span>

    <span data-ttu-id="066f3-221">![Tlačítko "Pozvat uživatele"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="066f3-221">!["Invite User" button](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="066f3-222">V **pozvat osoby** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="066f3-222">In the **Invite People** dialog box, perform the following steps:</span></span>

    <span data-ttu-id="066f3-223">![Dialogové okno "Pozvat osoby"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "pozvat osoby")</span><span class="sxs-lookup"><span data-stu-id="066f3-223">!["Invite People" dialog box](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="066f3-224">a.</span><span class="sxs-lookup"><span data-stu-id="066f3-224">a.</span></span> <span data-ttu-id="066f3-225">V **e-mailu** zadejte e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="066f3-225">In the **Email** box, type the email address of the Britta Simon account.</span></span>

    <span data-ttu-id="066f3-226">b.</span><span class="sxs-lookup"><span data-stu-id="066f3-226">b.</span></span> <span data-ttu-id="066f3-227">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="066f3-227">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="066f3-228">Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="066f3-228">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="066f3-229">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="066f3-229">Assign the Azure AD test user</span></span>
<span data-ttu-id="066f3-230">Povolte Britta Simon používat tak, že udělíte přístup k Clarizen Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="066f3-230">Enable Britta Simon to use Azure single sign-on by granting her access to Clarizen.</span></span>

![Přiřazené testovacího uživatele][200]

1. <span data-ttu-id="066f3-232">Ve službě Azure klikněte na portálu, otevřete zobrazit aplikace, přejděte do zobrazení adresáře **podnikové aplikace, které**a potom klikněte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="066f3-232">In the Azure portal, open the applications view, browse to the directory view, click **Enterprise applications**, and then click **All applications**.</span></span>

    ![Kliknutím na tlačítko "Podnikové aplikace" a "Všechny aplikace"][201]

2. <span data-ttu-id="066f3-234">V seznamu aplikací vyberte **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="066f3-234">In the applications list, select **Clarizen**.</span></span>

    ![V seznamu vyberete Clarizen](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. <span data-ttu-id="066f3-236">V levém podokně klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="066f3-236">In the left pane, click **Users and groups**.</span></span>

    ![Kliknutím na tlačítko "Uživatelé a skupiny"][202]

4. <span data-ttu-id="066f3-238">Klikněte **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="066f3-238">Click the **Add** button.</span></span> <span data-ttu-id="066f3-239">Potom v **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="066f3-239">Then, in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Tlačítko "Přidat" a "Přidat přiřazení" dialogových oken][203]

5. <span data-ttu-id="066f3-241">V **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="066f3-241">In the **Users and groups** dialog box, select **Britta Simon** in the list of users.</span></span>

6. <span data-ttu-id="066f3-242">V **uživatelů a skupin** dialogové okno, klikněte **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="066f3-242">In the **Users and groups** dialog box, click the **Select** button.</span></span>

7. <span data-ttu-id="066f3-243">V **přidat přiřazení** dialogové okno, klikněte **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="066f3-243">In the **Add Assignment** dialog box, click the **Assign** button.</span></span>

### <a name="test-single-sign-on"></a><span data-ttu-id="066f3-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="066f3-244">Test single sign-on</span></span>
<span data-ttu-id="066f3-245">Azure AD jeden přihlašování konfiguraci otestujte pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="066f3-245">Test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="066f3-246">Když kliknete na dlaždici Clarizen na přístupovém panelu, můžete by měl být automaticky přihlášeni do vaší aplikace Clarizen.</span><span class="sxs-lookup"><span data-stu-id="066f3-246">When you click the Clarizen tile in the Access Panel, you should be automatically signed in to your Clarizen application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="066f3-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="066f3-247">Additional resources</span></span>

* [<span data-ttu-id="066f3-248">Seznam kurzů k integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="066f3-248">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="066f3-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="066f3-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
