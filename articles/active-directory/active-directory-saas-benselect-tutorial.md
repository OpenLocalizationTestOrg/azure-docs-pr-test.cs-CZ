---
title: 'Kurz: Azure Active Directory integrace s BenSelect | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BenSelect."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffa17478-3ea1-4356-a289-545b5b9a4494
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: f8caa023da05863372b7ef92b47a92168445d741
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benselect"></a><span data-ttu-id="8ee51-103">Kurz: Azure Active Directory integrace s BenSelect</span><span class="sxs-lookup"><span data-stu-id="8ee51-103">Tutorial: Azure Active Directory integration with BenSelect</span></span>

<span data-ttu-id="8ee51-104">V tomto kurzu zjistěte, jak integrovat BenSelect s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8ee51-104">In this tutorial, you learn how to integrate BenSelect with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ee51-105">Integrace BenSelect s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8ee51-105">Integrating BenSelect with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8ee51-106">Můžete řídit ve službě Azure AD, který má přístup k BenSelect</span><span class="sxs-lookup"><span data-stu-id="8ee51-106">You can control in Azure AD who has access to BenSelect</span></span>
- <span data-ttu-id="8ee51-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BenSelect (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ee51-107">You can enable your users to automatically get signed-on to BenSelect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ee51-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8ee51-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8ee51-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ee51-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ee51-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8ee51-110">Prerequisites</span></span>

<span data-ttu-id="8ee51-111">Konfigurace integrace Azure AD s BenSelect, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="8ee51-111">To configure Azure AD integration with BenSelect, you need the following items:</span></span>

- <span data-ttu-id="8ee51-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ee51-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8ee51-113">BenSelect jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8ee51-113">A BenSelect single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8ee51-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ee51-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8ee51-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8ee51-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ee51-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8ee51-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8ee51-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ee51-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8ee51-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8ee51-118">Scenario description</span></span>
<span data-ttu-id="8ee51-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ee51-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ee51-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8ee51-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ee51-121">Přidání BenSelect z Galerie</span><span class="sxs-lookup"><span data-stu-id="8ee51-121">Adding BenSelect from the gallery</span></span>
2. <span data-ttu-id="8ee51-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ee51-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benselect-from-the-gallery"></a><span data-ttu-id="8ee51-123">Přidání BenSelect z Galerie</span><span class="sxs-lookup"><span data-stu-id="8ee51-123">Adding BenSelect from the gallery</span></span>
<span data-ttu-id="8ee51-124">Při konfiguraci integrace BenSelect do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS BenSelect z galerie.</span><span class="sxs-lookup"><span data-stu-id="8ee51-124">To configure the integration of BenSelect into Azure AD, you need to add BenSelect from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8ee51-125">**Pokud chcete přidat BenSelect z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ee51-125">**To add BenSelect from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8ee51-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ee51-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ee51-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8ee51-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8ee51-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ee51-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8ee51-133">Do vyhledávacího pole zadejte **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-133">In the search box, type **BenSelect**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_search.png)

5. <span data-ttu-id="8ee51-135">Na panelu výsledků vyberte **BenSelect**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8ee51-135">In the results panel, select **BenSelect**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ee51-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ee51-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8ee51-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BenSelect podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8ee51-138">In this section, you configure and test Azure AD single sign-on with BenSelect based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8ee51-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BenSelect je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8ee51-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BenSelect is to a user in Azure AD.</span></span> <span data-ttu-id="8ee51-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v BenSelect musí navázat.</span><span class="sxs-lookup"><span data-stu-id="8ee51-140">In other words, a link relationship between an Azure AD user and the related user in BenSelect needs to be established.</span></span>

<span data-ttu-id="8ee51-141">V BenSelect, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="8ee51-141">In BenSelect, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8ee51-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BenSelect, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="8ee51-142">To configure and test Azure AD single sign-on with BenSelect, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8ee51-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="8ee51-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8ee51-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ee51-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ee51-145">**[Vytvoření zkušebního uživatele BenSelect](#creating-a-benselect-test-user)**  – Pokud chcete mít protějšek Britta Simon v BenSelect propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="8ee51-145">**[Creating a BenSelect test user](#creating-a-benselect-test-user)** - to have a counterpart of Britta Simon in BenSelect that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8ee51-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ee51-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ee51-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="8ee51-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ee51-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ee51-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ee51-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BenSelect.</span><span class="sxs-lookup"><span data-stu-id="8ee51-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BenSelect application.</span></span>

<span data-ttu-id="8ee51-150">**Ke konfiguraci Azure AD jednotné přihlašování s BenSelect, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ee51-150">**To configure Azure AD single sign-on with BenSelect, perform the following steps:**</span></span>

1. <span data-ttu-id="8ee51-151">Na portálu Azure na **BenSelect** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-151">In the Azure portal, on the **BenSelect** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8ee51-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ee51-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_samlbase.png)

3. <span data-ttu-id="8ee51-155">Na **BenSelect domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ee51-155">On the **BenSelect Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_url.png)

    <span data-ttu-id="8ee51-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="8ee51-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8ee51-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="8ee51-158">This value is not real.</span></span> <span data-ttu-id="8ee51-159">Aktualizujte tuto hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8ee51-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="8ee51-160">Obraťte se na [tým podpory BenSelect](mailto:support@selerix.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8ee51-160">Contact [BenSelect support team](mailto:support@selerix.com) to get this value.</span></span>
 
4. <span data-ttu-id="8ee51-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="8ee51-161">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_certificate.png) 

5. <span data-ttu-id="8ee51-163">Aplikace BenSelect očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="8ee51-163">BenSelect application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="8ee51-164">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8ee51-164">Configure the following claims for this application.</span></span> <span data-ttu-id="8ee51-165">Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="8ee51-165">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="8ee51-166">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="8ee51-166">The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_06.png)

6. <span data-ttu-id="8ee51-168">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="8ee51-168">In the **User Attributes** section on the **Single sign-on** dialog:</span></span>

    <span data-ttu-id="8ee51-169">a.</span><span class="sxs-lookup"><span data-stu-id="8ee51-169">a.</span></span> <span data-ttu-id="8ee51-170">V **uživatelský identifikátor** rozevíracího seznamu vyberte **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-170">In the **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="8ee51-171">b.</span><span class="sxs-lookup"><span data-stu-id="8ee51-171">b.</span></span> <span data-ttu-id="8ee51-172">V **e-mailu** rozevíracího seznamu vyberte **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-172">In the **Mail** dropdown list, select **user.userprincipalname**.</span></span>

7. <span data-ttu-id="8ee51-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ee51-173">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8ee51-175">Na **BenSelect konfigurace** klikněte na tlačítko **konfigurace BenSelect** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8ee51-175">On the **BenSelect Configuration** section, click **Configure BenSelect** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8ee51-176">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8ee51-176">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_configure.png) 

9. <span data-ttu-id="8ee51-178">Konfigurace jednotného přihlašování na **BenSelect** straně, budete muset odeslat stažené **Certificate(Raw)** a **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory BenSelect](mailto:support@selerix.com).</span><span class="sxs-lookup"><span data-stu-id="8ee51-178">To configure single sign-on on **BenSelect** side, you need to send the downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [BenSelect support team](mailto:support@selerix.com).</span></span>

   >[!NOTE]
   ><span data-ttu-id="8ee51-179">Budete muset zmínili, že tato integrace vyžaduje algoritmus SHA256 (SHA1 není podporována.) Chcete-li nastavit jednotné přihlašování na příslušný server jako app2101 apod.</span><span class="sxs-lookup"><span data-stu-id="8ee51-179">You need to mention that this integration requires the SHA256 algorithm (SHA1 is not supported) to set the SSO on the appropriate server like app2101 etc.</span></span> 
   
> [!TIP]
> <span data-ttu-id="8ee51-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="8ee51-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8ee51-181">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="8ee51-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8ee51-182">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8ee51-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ee51-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ee51-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ee51-184">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ee51-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8ee51-186">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ee51-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8ee51-187">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ee51-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ee51-189">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ee51-191">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ee51-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ee51-193">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ee51-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ee51-195">a.</span><span class="sxs-lookup"><span data-stu-id="8ee51-195">a.</span></span> <span data-ttu-id="8ee51-196">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ee51-197">b.</span><span class="sxs-lookup"><span data-stu-id="8ee51-197">b.</span></span> <span data-ttu-id="8ee51-198">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ee51-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ee51-199">c.</span><span class="sxs-lookup"><span data-stu-id="8ee51-199">c.</span></span> <span data-ttu-id="8ee51-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8ee51-201">d.</span><span class="sxs-lookup"><span data-stu-id="8ee51-201">d.</span></span> <span data-ttu-id="8ee51-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-202">Click **Create**.</span></span>
 
### <a name="creating-a-benselect-test-user"></a><span data-ttu-id="8ee51-203">Vytvoření zkušebního uživatele BenSelect</span><span class="sxs-lookup"><span data-stu-id="8ee51-203">Creating a BenSelect test user</span></span>

<span data-ttu-id="8ee51-204">Cílem této části je vytvoření uživatele v BenSelect nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ee51-204">The objective of this section is to create a user called Britta Simon in BenSelect.</span></span> <span data-ttu-id="8ee51-205">Práce s [tým podpory BenSelect](mailto:support@selerix.com) přidat uživatele do BenSelect účtu.</span><span class="sxs-lookup"><span data-stu-id="8ee51-205">Work with [BenSelect support team](mailto:support@selerix.com) to add the users in the BenSelect account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8ee51-206">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ee51-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8ee51-207">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BenSelect.</span><span class="sxs-lookup"><span data-stu-id="8ee51-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BenSelect.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8ee51-209">**Pokud chcete přiřadit Britta Simon BenSelect, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="8ee51-209">**To assign Britta Simon to BenSelect, perform the following steps:**</span></span>

1. <span data-ttu-id="8ee51-210">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8ee51-212">V seznamu aplikací vyberte **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-212">In the applications list, select **BenSelect**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_app.png) 

3. <span data-ttu-id="8ee51-214">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8ee51-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8ee51-216">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ee51-216">Click **Add** button.</span></span> <span data-ttu-id="8ee51-217">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ee51-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8ee51-219">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8ee51-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8ee51-220">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ee51-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ee51-221">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ee51-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8ee51-222">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ee51-222">Testing single sign-on</span></span>

<span data-ttu-id="8ee51-223">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="8ee51-223">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="8ee51-224">Když kliknete na dlaždici BenSelect na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci BenSelect.</span><span class="sxs-lookup"><span data-stu-id="8ee51-224">When you click the BenSelect tile in the Access Panel, you should get automatically signed-on to your BenSelect application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ee51-225">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8ee51-225">Additional resources</span></span>

* [<span data-ttu-id="8ee51-226">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8ee51-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ee51-227">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8ee51-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_203.png

