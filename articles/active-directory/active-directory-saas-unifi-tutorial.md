---
title: 'Kurz: Azure Active Directory integrace s UNIFI | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a UNIFI."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 09074d4628825909f0bb961c8001e53fb06cf7c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a><span data-ttu-id="7d900-103">Kurz: Azure Active Directory integrace s UNIFI</span><span class="sxs-lookup"><span data-stu-id="7d900-103">Tutorial: Azure Active Directory integration with UNIFI</span></span>

<span data-ttu-id="7d900-104">V tomto kurzu zjistěte, jak integrovat UNIFI s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7d900-104">In this tutorial, you learn how to integrate UNIFI with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7d900-105">Integrace UNIFI s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7d900-105">Integrating UNIFI with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7d900-106">Můžete řídit ve službě Azure AD, který má přístup k UNIFI</span><span class="sxs-lookup"><span data-stu-id="7d900-106">You can control in Azure AD who has access to UNIFI</span></span>
- <span data-ttu-id="7d900-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k UNIFI (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d900-107">You can enable your users to automatically get signed-on to UNIFI (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7d900-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7d900-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7d900-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7d900-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d900-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7d900-110">Prerequisites</span></span>

<span data-ttu-id="7d900-111">Konfigurace integrace Azure AD s UNIFI, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="7d900-111">To configure Azure AD integration with UNIFI, you need the following items:</span></span>

- <span data-ttu-id="7d900-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d900-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7d900-113">UNIFI jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7d900-113">A UNIFI single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7d900-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7d900-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7d900-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7d900-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7d900-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7d900-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7d900-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7d900-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7d900-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7d900-118">Scenario description</span></span>
<span data-ttu-id="7d900-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7d900-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7d900-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7d900-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7d900-121">Přidání UNIFI z Galerie</span><span class="sxs-lookup"><span data-stu-id="7d900-121">Adding UNIFI from the gallery</span></span>
2. <span data-ttu-id="7d900-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d900-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-unifi-from-the-gallery"></a><span data-ttu-id="7d900-123">Přidání UNIFI z Galerie</span><span class="sxs-lookup"><span data-stu-id="7d900-123">Adding UNIFI from the gallery</span></span>
<span data-ttu-id="7d900-124">Při konfiguraci integrace UNIFI do služby Azure AD potřebujete přidat UNIFI z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7d900-124">To configure the integration of UNIFI into Azure AD, you need to add UNIFI from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7d900-125">**Pokud chcete přidat UNIFI z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7d900-125">**To add UNIFI from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7d900-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7d900-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7d900-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7d900-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7d900-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7d900-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7d900-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d900-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7d900-133">Do vyhledávacího pole zadejte **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="7d900-133">In the search box, type **UNIFI**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_search.png)

5. <span data-ttu-id="7d900-135">Na panelu výsledků vyberte **UNIFI**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7d900-135">In the results panel, select **UNIFI**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7d900-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d900-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7d900-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UNIFI podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7d900-138">In this section, you configure and test Azure AD single sign-on with UNIFI based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7d900-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v UNIFI je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d900-139">For single sign-on to work, Azure AD needs to know what the counterpart user in UNIFI is to a user in Azure AD.</span></span> <span data-ttu-id="7d900-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v UNIFI musí navázat.</span><span class="sxs-lookup"><span data-stu-id="7d900-140">In other words, a link relationship between an Azure AD user and the related user in UNIFI needs to be established.</span></span>

<span data-ttu-id="7d900-141">V UNIFI, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="7d900-141">In UNIFI, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7d900-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s UNIFI, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="7d900-142">To configure and test Azure AD single sign-on with UNIFI, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7d900-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="7d900-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7d900-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7d900-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7d900-145">**[Vytváření UNIFI testovací uživatel](#creating-a-unifi-test-user)**  – Pokud chcete mít protějšek Britta Simon v UNIFI propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d900-145">**[Creating a UNIFI test user](#creating-a-unifi-test-user)** - to have a counterpart of Britta Simon in UNIFI that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7d900-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d900-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7d900-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7d900-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7d900-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d900-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7d900-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci UNIFI.</span><span class="sxs-lookup"><span data-stu-id="7d900-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UNIFI application.</span></span>

<span data-ttu-id="7d900-150">**Ke konfiguraci Azure AD jednotné přihlašování s UNIFI, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7d900-150">**To configure Azure AD single sign-on with UNIFI, perform the following steps:**</span></span>

1. <span data-ttu-id="7d900-151">Na portálu Azure na **UNIFI** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7d900-151">In the Azure portal, on the **UNIFI** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7d900-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d900-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_samlbase.png)

3. <span data-ttu-id="7d900-155">Na **UNIFI domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="7d900-155">On the **UNIFI Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url1.png)

    <span data-ttu-id="7d900-157">V **identifikátor** textovému poli, zadejte hodnotu:`INVIEWlabs`</span><span class="sxs-lookup"><span data-stu-id="7d900-157">In the **Identifier** textbox, type the value: `INVIEWlabs`</span></span> 

4. <span data-ttu-id="7d900-158">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="7d900-158">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url2.png)

    <span data-ttu-id="7d900-160">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://app.discoverunifi.com/login`</span><span class="sxs-lookup"><span data-stu-id="7d900-160">In the **Sign-on URL** textbox, type the URL: `https://app.discoverunifi.com/login`</span></span>

5. <span data-ttu-id="7d900-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="7d900-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_certificate.png) 

6. <span data-ttu-id="7d900-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d900-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="7d900-165">Na **UNIFI konfigurace** klikněte na tlačítko **konfigurace UNIFI** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7d900-165">On the **UNIFI Configuration** section, click **Configure UNIFI** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7d900-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7d900-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_configure.png)

8. <span data-ttu-id="7d900-168">V okně prohlížeče jiný web, přihlaste se k vaší **UNIFI** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="7d900-168">In a different web browser window, sign on to your **UNIFI** company site as administrator.</span></span>

9. <span data-ttu-id="7d900-169">Klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7d900-169">Click on the **Users**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/app1.png) 

10. <span data-ttu-id="7d900-171">Klikněte na **přidat nového poskytovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="7d900-171">Click on the **Add New Identity Provider**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/app2.png)

11. <span data-ttu-id="7d900-173">V **přidat zprostředkovatele Identity** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7d900-173">In the **Add Identity Provider** section, perform the following steps:</span></span>   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/app3.png) 

    <span data-ttu-id="7d900-175">a.</span><span class="sxs-lookup"><span data-stu-id="7d900-175">a.</span></span> <span data-ttu-id="7d900-176">V **název zprostředkovatele** textovému poli, zadejte název zprostředkovatele Identity...</span><span class="sxs-lookup"><span data-stu-id="7d900-176">In the **Provider Name** textbox, type the name of the Identity Provider..</span></span>

    <span data-ttu-id="7d900-177">b.</span><span class="sxs-lookup"><span data-stu-id="7d900-177">b.</span></span> <span data-ttu-id="7d900-178">V **adresa URL poskytovatele** vložení textové pole **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7d900-178">In the the **Provider URL** textbox paste the **SAML Single Sign-On Service URL** value, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="7d900-179">c.</span><span class="sxs-lookup"><span data-stu-id="7d900-179">c.</span></span> <span data-ttu-id="7d900-180">Otevřete odebrat certifikát, který jste stáhli z portálu Azure v poznámkovém bloku **---BEGIN CERTIFICATE---** a **---KONCOVÝ certifikát,** značku a potom vložte zbývající obsah **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="7d900-180">Open the Certificate that you have downloaded from the Azure portal in notepad, remove the **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste the remaining content in the **Certificate** textbox.</span></span>

    <span data-ttu-id="7d900-181">d.</span><span class="sxs-lookup"><span data-stu-id="7d900-181">d.</span></span> <span data-ttu-id="7d900-182">Vyberte **je výchozí zprostředkovatel** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7d900-182">Select the **is Default Provider** checkbox.</span></span>

> [!TIP]
> <span data-ttu-id="7d900-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="7d900-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7d900-184">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="7d900-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7d900-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7d900-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7d900-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d900-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="7d900-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7d900-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7d900-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7d900-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7d900-190">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7d900-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7d900-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7d900-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7d900-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d900-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7d900-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7d900-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7d900-198">a.</span><span class="sxs-lookup"><span data-stu-id="7d900-198">a.</span></span> <span data-ttu-id="7d900-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7d900-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7d900-200">b.</span><span class="sxs-lookup"><span data-stu-id="7d900-200">b.</span></span> <span data-ttu-id="7d900-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7d900-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7d900-202">c.</span><span class="sxs-lookup"><span data-stu-id="7d900-202">c.</span></span> <span data-ttu-id="7d900-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7d900-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7d900-204">d.</span><span class="sxs-lookup"><span data-stu-id="7d900-204">d.</span></span> <span data-ttu-id="7d900-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7d900-205">Click **Create**.</span></span>
 
### <a name="creating-a-unifi-test-user"></a><span data-ttu-id="7d900-206">Vytvoření zkušebního uživatele UNIFI</span><span class="sxs-lookup"><span data-stu-id="7d900-206">Creating a UNIFI test user</span></span>

<span data-ttu-id="7d900-207">V této části vytvoříte názvem Britta Simon uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d900-207">In this section, you create a user called Britta Simon.</span></span> <span data-ttu-id="7d900-208">**UNIFI** podporuje zřizování automatické uživatelů, takže nejsou potřeba žádné ruční kroky.</span><span class="sxs-lookup"><span data-stu-id="7d900-208">**UNIFI** supports automatic user provisioning so no manual steps are required.</span></span> <span data-ttu-id="7d900-209">Uživatelé se vytvoří automaticky po úspěšném ověření z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d900-209">Users are created automatically after successful authentication from the Azure AD.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7d900-210">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d900-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7d900-211">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu UNIFI.</span><span class="sxs-lookup"><span data-stu-id="7d900-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UNIFI.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7d900-213">**Pokud chcete přiřadit Britta Simon UNIFI, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7d900-213">**To assign Britta Simon to UNIFI, perform the following steps:**</span></span>

1. <span data-ttu-id="7d900-214">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7d900-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7d900-216">V seznamu aplikací vyberte **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="7d900-216">In the applications list, select **UNIFI**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_app.png) 

3. <span data-ttu-id="7d900-218">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7d900-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7d900-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d900-220">Click **Add** button.</span></span> <span data-ttu-id="7d900-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d900-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7d900-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7d900-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7d900-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d900-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7d900-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d900-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7d900-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d900-226">Testing single sign-on</span></span>

<span data-ttu-id="7d900-227">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7d900-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7d900-228">Když kliknete na dlaždici UNIFI na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci UNIFI.</span><span class="sxs-lookup"><span data-stu-id="7d900-228">When you click the UNIFI tile in the Access Panel, you should get automatically signed-on to your UNIFI application.</span></span>
<span data-ttu-id="7d900-229">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7d900-229">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7d900-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7d900-230">Additional resources</span></span>

* [<span data-ttu-id="7d900-231">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d900-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7d900-232">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7d900-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_203.png

