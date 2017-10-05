---
title: 'Kurz: Azure Active Directory integrace s SAP NetWeaver | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP NetWeaver."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: ad4140eb1183094a67822ad92eabcd35101360b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a><span data-ttu-id="0311a-103">Kurz: Integrace Azure Active Directory se službou SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="0311a-103">Tutorial: Azure Active Directory integration with SAP NetWeaver</span></span>

<span data-ttu-id="0311a-104">V tomto kurzu zjistěte, jak integrovat SAP NetWeaver s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0311a-104">In this tutorial, you learn how to integrate SAP NetWeaver with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0311a-105">Integrace SAP NetWeaver s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0311a-105">Integrating SAP NetWeaver with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0311a-106">Můžete řídit ve službě Azure AD, který má přístup k SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="0311a-106">You can control in Azure AD who has access to SAP NetWeaver</span></span>
- <span data-ttu-id="0311a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k prostředí SAP NetWeaver (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0311a-107">You can enable your users to automatically get signed-on to SAP NetWeaver (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0311a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0311a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0311a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0311a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0311a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0311a-110">Prerequisites</span></span>

<span data-ttu-id="0311a-111">Konfigurace integrace Azure AD s SAP NetWeaver, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0311a-111">To configure Azure AD integration with SAP NetWeaver, you need the following items:</span></span>

- <span data-ttu-id="0311a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0311a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0311a-113">SAP NetWeaver jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0311a-113">An SAP NetWeaver single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0311a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0311a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0311a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0311a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0311a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0311a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0311a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0311a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0311a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0311a-118">Scenario description</span></span>
<span data-ttu-id="0311a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0311a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0311a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0311a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0311a-121">Přidání SAP NetWeaver z Galerie</span><span class="sxs-lookup"><span data-stu-id="0311a-121">Adding SAP NetWeaver from the gallery</span></span>
2. <span data-ttu-id="0311a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0311a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-netweaver-from-the-gallery"></a><span data-ttu-id="0311a-123">Přidání SAP NetWeaver z Galerie</span><span class="sxs-lookup"><span data-stu-id="0311a-123">Adding SAP NetWeaver from the gallery</span></span>
<span data-ttu-id="0311a-124">Při konfiguraci integrace SAP NetWeaver do služby Azure AD potřebujete přidat SAP NetWeaver z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0311a-124">To configure the integration of SAP NetWeaver into Azure AD, you need to add SAP NetWeaver from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0311a-125">**Pokud chcete přidat SAP NetWeaver z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0311a-125">**To add SAP NetWeaver from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0311a-126">V  **[portálu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0311a-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0311a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0311a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0311a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0311a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0311a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0311a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0311a-133">Do vyhledávacího pole zadejte **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="0311a-133">In the search box, type **SAP NetWeaver**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

5. <span data-ttu-id="0311a-135">Na panelu výsledků vyberte **SAP NetWeaver**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0311a-135">In the results panel, select **SAP NetWeaver**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0311a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0311a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0311a-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP NetWeaver podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0311a-138">In this section, you configure and test Azure AD single sign-on with SAP NetWeaver based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0311a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SAP NetWeaver je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0311a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAP NetWeaver is to a user in Azure AD.</span></span> <span data-ttu-id="0311a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SAP NetWeaver musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0311a-140">In other words, a link relationship between an Azure AD user and the related user in SAP NetWeaver needs to be established.</span></span>

<span data-ttu-id="0311a-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="0311a-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SAP NetWeaver.</span></span>

<span data-ttu-id="0311a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP NetWeaver, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0311a-142">To configure and test Azure AD single sign-on with SAP NetWeaver, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0311a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0311a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0311a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0311a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0311a-145">**[Vytváření testovacího uživatele SAP NetWeaver](#creating-an-sap-netweaver-test-user)**  – Pokud chcete mít protějšek Britta Simon v SAP NetWeaver propojenou s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0311a-145">**[Creating an SAP NetWeaver test user](#creating-an-sap-netweaver-test-user)** - to have a counterpart of Britta Simon in SAP NetWeaver that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0311a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0311a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0311a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0311a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0311a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0311a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0311a-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="0311a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAP NetWeaver application.</span></span>

<span data-ttu-id="0311a-150">**Ke konfiguraci Azure AD jednotné přihlašování s SAP NetWeaver, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0311a-150">**To configure Azure AD single sign-on with SAP NetWeaver, perform the following steps:**</span></span>

1. <span data-ttu-id="0311a-151">Na portálu Azure na **SAP NetWeaver** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0311a-151">In the Azure portal, on the **SAP NetWeaver** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0311a-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0311a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

3. <span data-ttu-id="0311a-155">Na **SAP NetWeaver domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0311a-155">On the **SAP NetWeaver Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    <span data-ttu-id="0311a-157">a.</span><span class="sxs-lookup"><span data-stu-id="0311a-157">a.</span></span> <span data-ttu-id="0311a-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="0311a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="0311a-159">b.</span><span class="sxs-lookup"><span data-stu-id="0311a-159">b.</span></span> <span data-ttu-id="0311a-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="0311a-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="0311a-161">c.</span><span class="sxs-lookup"><span data-stu-id="0311a-161">c.</span></span> <span data-ttu-id="0311a-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span><span class="sxs-lookup"><span data-stu-id="0311a-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="0311a-163">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="0311a-163">These values are not the real.</span></span> <span data-ttu-id="0311a-164">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="0311a-164">Update these values with the actual Identifier and Reply URL and Sign-On URL.</span></span> <span data-ttu-id="0311a-165">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="0311a-165">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="0311a-166">Obraťte se na [tým podpory SAP NetWeaver klienta](https://www.sap.com/support.html) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="0311a-166">Contact [SAP NetWeaver Client support team](https://www.sap.com/support.html) to get these values.</span></span> 

4. <span data-ttu-id="0311a-167">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0311a-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

5. <span data-ttu-id="0311a-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0311a-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="0311a-171">Na **SAP NetWeaver konfigurace** klikněte na tlačítko **konfigurace SAP NetWeaver** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0311a-171">On the **SAP NetWeaver Configuration** section, click **Configure SAP NetWeaver** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0311a-172">Kopírování **SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0311a-172">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

7. <span data-ttu-id="0311a-174">Konfigurace jednotného přihlašování na **SAP NetWeaver** straně, budete muset odeslat stažené **soubor XML s metadaty** a **SAML Entity ID** k [SAP NetWeaver podporu](https://www.sap.com/support.html).</span><span class="sxs-lookup"><span data-stu-id="0311a-174">To configure single sign-on on **SAP NetWeaver** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID** to [SAP NetWeaver support](https://www.sap.com/support.html).</span></span> 

> [!TIP]
> <span data-ttu-id="0311a-175">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="0311a-175">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0311a-176">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0311a-176">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0311a-177">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0311a-177">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0311a-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0311a-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="0311a-179">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0311a-179">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0311a-181">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0311a-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0311a-182">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0311a-182">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0311a-184">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0311a-184">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0311a-186">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0311a-186">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0311a-188">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0311a-188">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0311a-190">a.</span><span class="sxs-lookup"><span data-stu-id="0311a-190">a.</span></span> <span data-ttu-id="0311a-191">V **název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0311a-191">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="0311a-192">b.</span><span class="sxs-lookup"><span data-stu-id="0311a-192">b.</span></span> <span data-ttu-id="0311a-193">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0311a-193">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="0311a-194">c.</span><span class="sxs-lookup"><span data-stu-id="0311a-194">c.</span></span> <span data-ttu-id="0311a-195">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0311a-195">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0311a-196">d.</span><span class="sxs-lookup"><span data-stu-id="0311a-196">d.</span></span> <span data-ttu-id="0311a-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0311a-197">Click **Create**.</span></span>
 
### <a name="creating-an-sap-netweaver-test-user"></a><span data-ttu-id="0311a-198">Vytváření testovacího uživatele SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="0311a-198">Creating an SAP NetWeaver test user</span></span>

<span data-ttu-id="0311a-199">V této části vytvoříte volal Britta Simon v SAP NetWeaver uživatele.</span><span class="sxs-lookup"><span data-stu-id="0311a-199">In this section, you create a user called Britta Simon in SAP NetWeaver.</span></span> <span data-ttu-id="0311a-200">Práce s vaší [SAP NetWeaver podporu](https://www.sap.com/support.html) přidat uživatele do SAP NetWeaver platformy.</span><span class="sxs-lookup"><span data-stu-id="0311a-200">Work with your [SAP NetWeaver support](https://www.sap.com/support.html) to add the users in the SAP NetWeaver platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0311a-201">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0311a-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0311a-202">V této části povolíte Britta Simon používat tak, že udělíte přístup k SAP NetWeaver Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0311a-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAP NetWeaver.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0311a-204">**Pokud chcete přiřadit Britta Simon SAP NetWeaver, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0311a-204">**To assign Britta Simon to SAP NetWeaver, perform the following steps:**</span></span>

1. <span data-ttu-id="0311a-205">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0311a-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0311a-207">V seznamu aplikací vyberte **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="0311a-207">In the applications list, select **SAP NetWeaver**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. <span data-ttu-id="0311a-209">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0311a-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0311a-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0311a-211">Click **Add** button.</span></span> <span data-ttu-id="0311a-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0311a-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0311a-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0311a-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0311a-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0311a-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0311a-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0311a-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0311a-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0311a-217">Testing single sign-on</span></span>

<span data-ttu-id="0311a-218">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0311a-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0311a-219">Když kliknete na dlaždici SAP NetWeaver na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="0311a-219">When you click the SAP NetWeaver tile in the Access Panel, you should get automatically signed-on to your SAP NetWeaver application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0311a-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0311a-220">Additional resources</span></span>

* [<span data-ttu-id="0311a-221">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0311a-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0311a-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0311a-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_203.png

