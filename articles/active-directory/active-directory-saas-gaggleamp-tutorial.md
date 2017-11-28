---
title: 'Kurz: Azure Active Directory integrace s GaggleAMP | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a GaggleAMP."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: c591cf99aecc4153e378c42a530b80deeca63158
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a><span data-ttu-id="631dd-103">Kurz: Azure Active Directory integrace s GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="631dd-103">Tutorial: Azure Active Directory integration with GaggleAMP</span></span>

<span data-ttu-id="631dd-104">V tomto kurzu zjistěte, jak integrovat GaggleAMP s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="631dd-104">In this tutorial, you learn how to integrate GaggleAMP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="631dd-105">Integrace GaggleAMP s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="631dd-105">Integrating GaggleAMP with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="631dd-106">Můžete řídit ve službě Azure AD, který má přístup k GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="631dd-106">You can control in Azure AD who has access to GaggleAMP</span></span>
- <span data-ttu-id="631dd-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k GaggleAMP (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="631dd-107">You can enable your users to automatically get signed-on to GaggleAMP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="631dd-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="631dd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="631dd-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="631dd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="631dd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="631dd-110">Prerequisites</span></span>

<span data-ttu-id="631dd-111">Konfigurace integrace Azure AD s GaggleAMP, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="631dd-111">To configure Azure AD integration with GaggleAMP, you need the following items:</span></span>

- <span data-ttu-id="631dd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="631dd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="631dd-113">GaggleAMP jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="631dd-113">A GaggleAMP single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="631dd-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="631dd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="631dd-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="631dd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="631dd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="631dd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="631dd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="631dd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="631dd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="631dd-118">Scenario description</span></span>
<span data-ttu-id="631dd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="631dd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="631dd-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="631dd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="631dd-121">Přidání GaggleAMP z Galerie</span><span class="sxs-lookup"><span data-stu-id="631dd-121">Adding GaggleAMP from the gallery</span></span>
2. <span data-ttu-id="631dd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="631dd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gaggleamp-from-the-gallery"></a><span data-ttu-id="631dd-123">Přidání GaggleAMP z Galerie</span><span class="sxs-lookup"><span data-stu-id="631dd-123">Adding GaggleAMP from the gallery</span></span>
<span data-ttu-id="631dd-124">Při konfiguraci integrace GaggleAMP do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS GaggleAMP z galerie.</span><span class="sxs-lookup"><span data-stu-id="631dd-124">To configure the integration of GaggleAMP into Azure AD, you need to add GaggleAMP from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="631dd-125">**Pokud chcete přidat GaggleAMP z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="631dd-125">**To add GaggleAMP from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="631dd-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="631dd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="631dd-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="631dd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="631dd-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="631dd-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="631dd-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="631dd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="631dd-133">Do vyhledávacího pole zadejte **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="631dd-133">In the search box, type **GaggleAMP**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_search.png)

5. <span data-ttu-id="631dd-135">Na panelu výsledků vyberte **GaggleAMP**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="631dd-135">In the results panel, select **GaggleAMP**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="631dd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="631dd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="631dd-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s GaggleAMP podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="631dd-138">In this section, you configure and test Azure AD single sign-on with GaggleAMP based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="631dd-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v GaggleAMP je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="631dd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in GaggleAMP is to a user in Azure AD.</span></span> <span data-ttu-id="631dd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v GaggleAMP musí navázat.</span><span class="sxs-lookup"><span data-stu-id="631dd-140">In other words, a link relationship between an Azure AD user and the related user in GaggleAMP needs to be established.</span></span>

<span data-ttu-id="631dd-141">V GaggleAMP, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="631dd-141">In GaggleAMP, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="631dd-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s GaggleAMP, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="631dd-142">To configure and test Azure AD single sign-on with GaggleAMP, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="631dd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="631dd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="631dd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="631dd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="631dd-145">**[Vytvoření zkušebního uživatele GaggleAMP](#creating-a-gaggleamp-test-user)**  – Pokud chcete mít protějšek Britta Simon v GaggleAMP propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="631dd-145">**[Creating a GaggleAMP test user](#creating-a-gaggleamp-test-user)** - to have a counterpart of Britta Simon in GaggleAMP that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="631dd-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631dd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="631dd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="631dd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="631dd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="631dd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="631dd-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="631dd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your GaggleAMP application.</span></span>

<span data-ttu-id="631dd-150">**Ke konfiguraci Azure AD jednotné přihlašování s GaggleAMP, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="631dd-150">**To configure Azure AD single sign-on with GaggleAMP, perform the following steps:**</span></span>

1. <span data-ttu-id="631dd-151">Na portálu Azure na **GaggleAMP** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="631dd-151">In the Azure portal, on the **GaggleAMP** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="631dd-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631dd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

3. <span data-ttu-id="631dd-155">Na **GaggleAMP domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="631dd-155">On the **GaggleAMP Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     <span data-ttu-id="631dd-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.gaggleamp.com`</span><span class="sxs-lookup"><span data-stu-id="631dd-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.gaggleamp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="631dd-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="631dd-158">The value is not real.</span></span> <span data-ttu-id="631dd-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="631dd-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="631dd-160">Obraťte se na [tým podpory GaggleAMP klienta](mailto:sales@gaggleamp.com) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="631dd-160">Contact [GaggleAMP Client support team](mailto:sales@gaggleamp.com) to get the value.</span></span> 
 
4. <span data-ttu-id="631dd-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="631dd-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

5. <span data-ttu-id="631dd-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="631dd-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="631dd-165">Na **GaggleAMP konfigurace** klikněte na tlačítko **konfigurace GaggleAMP** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="631dd-165">On the **GaggleAMP Configuration** section, click **Configure GaggleAMP** to open **Configure sign-on** window.</span></span> <span data-ttu-id="631dd-166">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="631dd-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

7. <span data-ttu-id="631dd-168">V instanci jiný prohlížeč, přejít na stránku jednotné přihlašování SAML vytvořené pro vám Gaggle podporu team (například: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span><span class="sxs-lookup"><span data-stu-id="631dd-168">In another browser instance, navigate to the SAML SSO page created for you by the Gaggle support team (for example: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span></span>

8. <span data-ttu-id="631dd-169">Na vaše **jednotné přihlašování SAML** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="631dd-169">On your **SAML SSO** page, perform the following steps:</span></span>  
   
    ![GaggleAMP jednotné přihlášení](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png) 
 
    <span data-ttu-id="631dd-171">a.</span><span class="sxs-lookup"><span data-stu-id="631dd-171">a.</span></span> <span data-ttu-id="631dd-172">V **vystavitele zprostředkovatele Identity** textovému poli, vložte hodnotu **URL vystavitele** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="631dd-172">In the **Identity Provider Issuer** textbox, paste the value of **Issuer URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="631dd-173">b.</span><span class="sxs-lookup"><span data-stu-id="631dd-173">b.</span></span> <span data-ttu-id="631dd-174">V **Identity zprostředkovatele jeden přihlašovací adresa URL** textovému poli, vložte hodnotu **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="631dd-174">In the **Identity Provider Single Sign-On URL** textbox, paste the  value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="631dd-175">c.</span><span class="sxs-lookup"><span data-stu-id="631dd-175">c.</span></span> <span data-ttu-id="631dd-176">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="631dd-176">Click **Save**</span></span>      

    <span data-ttu-id="631dd-177">d.</span><span class="sxs-lookup"><span data-stu-id="631dd-177">d.</span></span> <span data-ttu-id="631dd-178">Odeslat **certifikátu (Base64)** certifikát do vaší [tým podpory GaggleAMP](mailto:sales@gaggleamp.com).</span><span class="sxs-lookup"><span data-stu-id="631dd-178">Send the **Certificate (Base64)** certificate to your [GaggleAMP support team](mailto:sales@gaggleamp.com).</span></span>

> [!TIP]
> <span data-ttu-id="631dd-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="631dd-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="631dd-180">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="631dd-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="631dd-181">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="631dd-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="631dd-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="631dd-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="631dd-183">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="631dd-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="631dd-185">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="631dd-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="631dd-186">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="631dd-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="631dd-188">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="631dd-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="631dd-190">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="631dd-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="631dd-192">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="631dd-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="631dd-194">a.</span><span class="sxs-lookup"><span data-stu-id="631dd-194">a.</span></span> <span data-ttu-id="631dd-195">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="631dd-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="631dd-196">b.</span><span class="sxs-lookup"><span data-stu-id="631dd-196">b.</span></span> <span data-ttu-id="631dd-197">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="631dd-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="631dd-198">c.</span><span class="sxs-lookup"><span data-stu-id="631dd-198">c.</span></span> <span data-ttu-id="631dd-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="631dd-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="631dd-200">d.</span><span class="sxs-lookup"><span data-stu-id="631dd-200">d.</span></span> <span data-ttu-id="631dd-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="631dd-201">Click **Create**.</span></span>
 
### <a name="creating-a-gaggleamp-test-user"></a><span data-ttu-id="631dd-202">Vytvoření zkušebního uživatele GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="631dd-202">Creating a GaggleAMP test user</span></span>

<span data-ttu-id="631dd-203">Cílem této části je vytvoření uživatele v GaggleAMP nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="631dd-203">The objective of this section is to create a user called Britta Simon in GaggleAMP.</span></span> <span data-ttu-id="631dd-204">GaggleAMP podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="631dd-204">GaggleAMP supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="631dd-205">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="631dd-205">There is no action item for you in this section.</span></span> <span data-ttu-id="631dd-206">Nový uživatel se vytvoří během pokusu o přístup k GaggleAMP, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="631dd-206">A new user is created during an attempt to access GaggleAMP if it doesn't exist yet.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="631dd-207">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="631dd-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="631dd-208">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="631dd-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to GaggleAMP.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="631dd-210">**Pokud chcete přiřadit Britta Simon GaggleAMP, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="631dd-210">**To assign Britta Simon to GaggleAMP, perform the following steps:**</span></span>

1. <span data-ttu-id="631dd-211">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="631dd-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="631dd-213">V seznamu aplikací vyberte **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="631dd-213">In the applications list, select **GaggleAMP**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

3. <span data-ttu-id="631dd-215">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="631dd-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="631dd-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="631dd-217">Click **Add** button.</span></span> <span data-ttu-id="631dd-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="631dd-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="631dd-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="631dd-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="631dd-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="631dd-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="631dd-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="631dd-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="631dd-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="631dd-223">Testing single sign-on</span></span>

<span data-ttu-id="631dd-224">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="631dd-224">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="631dd-225">Když kliknete na dlaždici GaggleAMP na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="631dd-225">When you click the GaggleAMP tile in the Access Panel, you should get automatically signed-on to your GaggleAMP application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="631dd-226">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="631dd-226">Additional resources</span></span>

* [<span data-ttu-id="631dd-227">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="631dd-227">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="631dd-228">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="631dd-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png

