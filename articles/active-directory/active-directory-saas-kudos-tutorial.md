---
title: 'Kurz: Azure Active Directory integrace s Kudos | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kudos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 353798fcfd4ad7ce017fc2fddf4110715db3ace2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a><span data-ttu-id="bd50c-103">Kurz: Azure Active Directory integrace s Kudos</span><span class="sxs-lookup"><span data-stu-id="bd50c-103">Tutorial: Azure Active Directory integration with Kudos</span></span>

<span data-ttu-id="bd50c-104">V tomto kurzu zjistěte, jak integrovat Kudos s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bd50c-104">In this tutorial, you learn how to integrate Kudos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bd50c-105">Integrace Kudos s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bd50c-105">Integrating Kudos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bd50c-106">Můžete řídit ve službě Azure AD, který má přístup k Kudos</span><span class="sxs-lookup"><span data-stu-id="bd50c-106">You can control in Azure AD who has access to Kudos</span></span>
- <span data-ttu-id="bd50c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Kudos (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd50c-107">You can enable your users to automatically get signed-on to Kudos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bd50c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bd50c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bd50c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bd50c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd50c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bd50c-110">Prerequisites</span></span>

<span data-ttu-id="bd50c-111">Konfigurace integrace Azure AD s Kudos, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="bd50c-111">To configure Azure AD integration with Kudos, you need the following items:</span></span>

- <span data-ttu-id="bd50c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd50c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bd50c-113">Kudos jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bd50c-113">A Kudos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bd50c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd50c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bd50c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bd50c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bd50c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bd50c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bd50c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd50c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bd50c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bd50c-118">Scenario description</span></span>
<span data-ttu-id="bd50c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd50c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bd50c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bd50c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bd50c-121">Přidání Kudos z Galerie</span><span class="sxs-lookup"><span data-stu-id="bd50c-121">Adding Kudos from the gallery</span></span>
2. <span data-ttu-id="bd50c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bd50c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kudos-from-the-gallery"></a><span data-ttu-id="bd50c-123">Přidání Kudos z Galerie</span><span class="sxs-lookup"><span data-stu-id="bd50c-123">Adding Kudos from the gallery</span></span>
<span data-ttu-id="bd50c-124">Při konfiguraci integrace Kudos do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Kudos z galerie.</span><span class="sxs-lookup"><span data-stu-id="bd50c-124">To configure the integration of Kudos into Azure AD, you need to add Kudos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bd50c-125">**Pokud chcete přidat Kudos z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bd50c-125">**To add Kudos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bd50c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bd50c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bd50c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bd50c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="bd50c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bd50c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="bd50c-133">Do vyhledávacího pole zadejte **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-133">In the search box, type **Kudos**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_search.png)

5. <span data-ttu-id="bd50c-135">Na panelu výsledků vyberte **Kudos**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bd50c-135">In the results panel, select **Kudos**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bd50c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bd50c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bd50c-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kudos podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="bd50c-138">In this section, you configure and test Azure AD single sign-on with Kudos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bd50c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Kudos je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd50c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kudos is to a user in Azure AD.</span></span> <span data-ttu-id="bd50c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Kudos musí navázat.</span><span class="sxs-lookup"><span data-stu-id="bd50c-140">In other words, a link relationship between an Azure AD user and the related user in Kudos needs to be established.</span></span>

<span data-ttu-id="bd50c-141">V Kudos, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="bd50c-141">In Kudos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bd50c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kudos, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="bd50c-142">To configure and test Azure AD single sign-on with Kudos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bd50c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="bd50c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bd50c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd50c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bd50c-145">**[Vytvoření zkušebního uživatele Kudos](#creating-a-kudos-test-user)**  – Pokud chcete mít protějšek Britta Simon v Kudos propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="bd50c-145">**[Creating a Kudos test user](#creating-a-kudos-test-user)** - to have a counterpart of Britta Simon in Kudos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bd50c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bd50c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bd50c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bd50c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bd50c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bd50c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bd50c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Kudos.</span><span class="sxs-lookup"><span data-stu-id="bd50c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kudos application.</span></span>

<span data-ttu-id="bd50c-150">**Ke konfiguraci Azure AD jednotné přihlašování s Kudos, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bd50c-150">**To configure Azure AD single sign-on with Kudos, perform the following steps:**</span></span>

1. <span data-ttu-id="bd50c-151">Na portálu Azure na **Kudos** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-151">In the Azure portal, on the **Kudos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="bd50c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bd50c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_samlbase.png)

3. <span data-ttu-id="bd50c-155">Na **Kudos domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bd50c-155">On the **Kudos Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_url.png)

    <span data-ttu-id="bd50c-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company>.kudosnow.com`</span><span class="sxs-lookup"><span data-stu-id="bd50c-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.kudosnow.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="bd50c-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="bd50c-158">This value is not real.</span></span> <span data-ttu-id="bd50c-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bd50c-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="bd50c-160">Obraťte se na [tým podpory Kudos klienta](http://success.kudosnow.com/home) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bd50c-160">Contact [Kudos Client support team](http://success.kudosnow.com/home) to get this value.</span></span> 
 
4. <span data-ttu-id="bd50c-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="bd50c-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_certificate.png) 

5. <span data-ttu-id="bd50c-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bd50c-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bd50c-165">Na **Kudos konfigurace** klikněte na tlačítko **konfigurace Kudos** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="bd50c-165">On the **Kudos Configuration** section, click **Configure Kudos** to open **Configure sign-on** window.</span></span> <span data-ttu-id="bd50c-166">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="bd50c-166">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_configure.png) 

7. <span data-ttu-id="bd50c-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Kudos.</span><span class="sxs-lookup"><span data-stu-id="bd50c-168">In a different web browser window, log into your Kudos company site as an administrator.</span></span>

8. <span data-ttu-id="bd50c-169">V nabídce v horní části, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-169">In the menu on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="bd50c-170">![Nastavení](./media/active-directory-saas-kudos-tutorial/ic787806.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="bd50c-170">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

9. <span data-ttu-id="bd50c-171">Klikněte na tlačítko **integrace \> jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-171">Click **Integrations \> SSO**.</span></span>

10. <span data-ttu-id="bd50c-172">V **jednotného přihlašování k** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bd50c-172">In the **SSO** section, perform the following steps:</span></span>
   
    <span data-ttu-id="bd50c-173">![JEDNOTNÉ PŘIHLAŠOVÁNÍ](./media/active-directory-saas-kudos-tutorial/ic787807.png "JEDNOTNÉHO PŘIHLAŠOVÁNÍ")</span><span class="sxs-lookup"><span data-stu-id="bd50c-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "SSO")</span></span>
   
    <span data-ttu-id="bd50c-174">a.</span><span class="sxs-lookup"><span data-stu-id="bd50c-174">a.</span></span> <span data-ttu-id="bd50c-175">V **přihlásit na adrese URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bd50c-175">In **Sign on URL** textbox, paste the value of  **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="bd50c-176">b.</span><span class="sxs-lookup"><span data-stu-id="bd50c-176">b.</span></span> <span data-ttu-id="bd50c-177">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textbox</span><span class="sxs-lookup"><span data-stu-id="bd50c-177">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 certificate** textbox</span></span>
   
    <span data-ttu-id="bd50c-178">c.</span><span class="sxs-lookup"><span data-stu-id="bd50c-178">c.</span></span> <span data-ttu-id="bd50c-179">V **odhlášení na adresu URL**, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bd50c-179">In **Logout To URL**, paste the value of  **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="bd50c-180">d.</span><span class="sxs-lookup"><span data-stu-id="bd50c-180">d.</span></span> <span data-ttu-id="bd50c-181">V **vaše adresa URL Kudos** textovému poli, zadejte název vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="bd50c-181">In the **Your Kudos URL** textbox, type your company name.</span></span>
   
    <span data-ttu-id="bd50c-182">e.</span><span class="sxs-lookup"><span data-stu-id="bd50c-182">e.</span></span> <span data-ttu-id="bd50c-183">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="bd50c-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="bd50c-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bd50c-185">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="bd50c-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bd50c-186">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bd50c-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bd50c-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd50c-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="bd50c-188">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd50c-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="bd50c-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bd50c-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bd50c-191">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bd50c-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bd50c-193">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bd50c-195">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bd50c-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bd50c-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bd50c-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bd50c-199">a.</span><span class="sxs-lookup"><span data-stu-id="bd50c-199">a.</span></span> <span data-ttu-id="bd50c-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bd50c-201">b.</span><span class="sxs-lookup"><span data-stu-id="bd50c-201">b.</span></span> <span data-ttu-id="bd50c-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bd50c-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bd50c-203">c.</span><span class="sxs-lookup"><span data-stu-id="bd50c-203">c.</span></span> <span data-ttu-id="bd50c-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bd50c-205">d.</span><span class="sxs-lookup"><span data-stu-id="bd50c-205">d.</span></span> <span data-ttu-id="bd50c-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-206">Click **Create**.</span></span>
 
### <a name="creating-a-kudos-test-user"></a><span data-ttu-id="bd50c-207">Vytvoření zkušebního uživatele Kudos</span><span class="sxs-lookup"><span data-stu-id="bd50c-207">Creating a Kudos test user</span></span>

<span data-ttu-id="bd50c-208">Pokud chcete povolit uživatelům Azure AD přihlášení do Kudos, musí být zřízená do Kudos.</span><span class="sxs-lookup"><span data-stu-id="bd50c-208">In order to enable Azure AD users to log into Kudos, they must be provisioned into Kudos.</span></span> 

<span data-ttu-id="bd50c-209">V případě Kudos zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="bd50c-209">In the case of Kudos, provisioning is a manual task.</span></span>

<span data-ttu-id="bd50c-210">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bd50c-210">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="bd50c-211">Přihlaste se k vaší **Kudos** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="bd50c-211">Log in to your **Kudos** company site as administrator.</span></span>

2. <span data-ttu-id="bd50c-212">V nabídce v horní části, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-212">In the menu on the top, click **Settings**.</span></span>
   
   <span data-ttu-id="bd50c-213">![Nastavení](./media/active-directory-saas-kudos-tutorial/ic787806.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="bd50c-213">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

3. <span data-ttu-id="bd50c-214">Klikněte na tlačítko **uživatele správce**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-214">Click **User Admin**.</span></span>

4. <span data-ttu-id="bd50c-215">Klikněte **uživatelé** a pak klikněte **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-215">Click the **Users** tab, and then click **Add a User**.</span></span>
   
   <span data-ttu-id="bd50c-216">![Uživatel správce](./media/active-directory-saas-kudos-tutorial/ic787809.png "správce uživatele")</span><span class="sxs-lookup"><span data-stu-id="bd50c-216">![User Admin](./media/active-directory-saas-kudos-tutorial/ic787809.png "User Admin")</span></span>

5. <span data-ttu-id="bd50c-217">V **přidat uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bd50c-217">In the **Add a User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="bd50c-218">![Přidat uživatele](./media/active-directory-saas-kudos-tutorial/ic787810.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="bd50c-218">![Add a User](./media/active-directory-saas-kudos-tutorial/ic787810.png "Add a User")</span></span>
   
    <span data-ttu-id="bd50c-219">a.</span><span class="sxs-lookup"><span data-stu-id="bd50c-219">a.</span></span> <span data-ttu-id="bd50c-220">Typ **křestní jméno**, **příjmení**, **e-mailu** a další podrobnosti platný účet služby Azure Active Directory chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="bd50c-220">Type the **First Name**, **Last Name**, **Email** and other details of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="bd50c-221">b.</span><span class="sxs-lookup"><span data-stu-id="bd50c-221">b.</span></span> <span data-ttu-id="bd50c-222">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-222">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="bd50c-223">Můžete použít všechny ostatní Kudos uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Kudos zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="bd50c-223">You can use any other Kudos user account creation tools or APIs provided by Kudos to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bd50c-224">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd50c-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bd50c-225">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Kudos.</span><span class="sxs-lookup"><span data-stu-id="bd50c-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kudos.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="bd50c-227">**Pokud chcete přiřadit Britta Simon Kudos, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bd50c-227">**To assign Britta Simon to Kudos, perform the following steps:**</span></span>

1. <span data-ttu-id="bd50c-228">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bd50c-230">V seznamu aplikací vyberte **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-230">In the applications list, select **Kudos**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_app.png) 

3. <span data-ttu-id="bd50c-232">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bd50c-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="bd50c-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bd50c-234">Click **Add** button.</span></span> <span data-ttu-id="bd50c-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bd50c-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="bd50c-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bd50c-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bd50c-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bd50c-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bd50c-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bd50c-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bd50c-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bd50c-240">Testing single sign-on</span></span>

<span data-ttu-id="bd50c-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bd50c-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bd50c-242">Když kliknete na dlaždici Kudos na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Kudos.</span><span class="sxs-lookup"><span data-stu-id="bd50c-242">When you click the Kudos tile in the Access Panel, you should get automatically signed-on to your Kudos application.</span></span> <span data-ttu-id="bd50c-243">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bd50c-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd50c-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bd50c-244">Additional resources</span></span>

* [<span data-ttu-id="bd50c-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bd50c-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bd50c-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bd50c-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_203.png

