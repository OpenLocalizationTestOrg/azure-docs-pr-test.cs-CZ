---
title: 'Kurz: Azure Active Directory integrace s PerformanceCentre | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PerformanceCentre."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: e86adaf4bd9b4752f2aece8207a8a423ec5590a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a><span data-ttu-id="9593d-103">Kurz: Azure Active Directory integrace s PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="9593d-103">Tutorial: Azure Active Directory integration with PerformanceCentre</span></span>

<span data-ttu-id="9593d-104">V tomto kurzu zjistěte, jak integrovat PerformanceCentre s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9593d-104">In this tutorial, you learn how to integrate PerformanceCentre with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9593d-105">Integrace PerformanceCentre s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9593d-105">Integrating PerformanceCentre with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9593d-106">Můžete řídit ve službě Azure AD, který má přístup k PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="9593d-106">You can control in Azure AD who has access to PerformanceCentre</span></span>
- <span data-ttu-id="9593d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k PerformanceCentre (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9593d-107">You can enable your users to automatically get signed-on to PerformanceCentre (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9593d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9593d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9593d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9593d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9593d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9593d-110">Prerequisites</span></span>

<span data-ttu-id="9593d-111">Konfigurace integrace Azure AD s PerformanceCentre, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9593d-111">To configure Azure AD integration with PerformanceCentre, you need the following items:</span></span>

- <span data-ttu-id="9593d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9593d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9593d-113">PerformanceCentre jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9593d-113">A PerformanceCentre single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9593d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9593d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9593d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9593d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9593d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9593d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9593d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9593d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9593d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9593d-118">Scenario description</span></span>
<span data-ttu-id="9593d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9593d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9593d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9593d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9593d-121">Přidání PerformanceCentre z Galerie</span><span class="sxs-lookup"><span data-stu-id="9593d-121">Adding PerformanceCentre from the gallery</span></span>
2. <span data-ttu-id="9593d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9593d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-performancecentre-from-the-gallery"></a><span data-ttu-id="9593d-123">Přidání PerformanceCentre z Galerie</span><span class="sxs-lookup"><span data-stu-id="9593d-123">Adding PerformanceCentre from the gallery</span></span>
<span data-ttu-id="9593d-124">Při konfiguraci integrace PerformanceCentre do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS PerformanceCentre z galerie.</span><span class="sxs-lookup"><span data-stu-id="9593d-124">To configure the integration of PerformanceCentre into Azure AD, you need to add PerformanceCentre from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9593d-125">**Pokud chcete přidat PerformanceCentre z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9593d-125">**To add PerformanceCentre from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9593d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9593d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9593d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9593d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9593d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9593d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9593d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9593d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9593d-133">Do vyhledávacího pole zadejte **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="9593d-133">In the search box, type **PerformanceCentre**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_search.png)

5. <span data-ttu-id="9593d-135">Na panelu výsledků vyberte **PerformanceCentre**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9593d-135">In the results panel, select **PerformanceCentre**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9593d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9593d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9593d-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PerformanceCentre podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9593d-138">In this section, you configure and test Azure AD single sign-on with PerformanceCentre based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9593d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v PerformanceCentre je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9593d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PerformanceCentre is to a user in Azure AD.</span></span> <span data-ttu-id="9593d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v PerformanceCentre musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9593d-140">In other words, a link relationship between an Azure AD user and the related user in PerformanceCentre needs to be established.</span></span>

<span data-ttu-id="9593d-141">V PerformanceCentre, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="9593d-141">In PerformanceCentre, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9593d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PerformanceCentre, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9593d-142">To configure and test Azure AD single sign-on with PerformanceCentre, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9593d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9593d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9593d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9593d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9593d-145">**[Vytvoření zkušebního uživatele PerformanceCentre](#creating-a-performancecentre-test-user)**  – Pokud chcete mít protějšek Britta Simon v PerformanceCentre propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9593d-145">**[Creating a PerformanceCentre test user](#creating-a-performancecentre-test-user)** - to have a counterpart of Britta Simon in PerformanceCentre that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9593d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9593d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9593d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9593d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9593d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9593d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9593d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="9593d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PerformanceCentre application.</span></span>

<span data-ttu-id="9593d-150">**Ke konfiguraci Azure AD jednotné přihlašování s PerformanceCentre, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9593d-150">**To configure Azure AD single sign-on with PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="9593d-151">Na portálu Azure na **PerformanceCentre** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9593d-151">In the Azure portal, on the **PerformanceCentre** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9593d-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9593d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. <span data-ttu-id="9593d-155">Na **PerformanceCentre domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9593d-155">On the **PerformanceCentre Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_url.png)

    <span data-ttu-id="9593d-157">a.</span><span class="sxs-lookup"><span data-stu-id="9593d-157">a.</span></span> <span data-ttu-id="9593d-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://companyname.performancecentre.com/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="9593d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://companyname.performancecentre.com/saml/SSO`</span></span>

    <span data-ttu-id="9593d-159">b.</span><span class="sxs-lookup"><span data-stu-id="9593d-159">b.</span></span> <span data-ttu-id="9593d-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://companyname.performancecentre.com`</span><span class="sxs-lookup"><span data-stu-id="9593d-160">In the **Identifier** textbox, type a URL using the following pattern: `http://companyname.performancecentre.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9593d-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9593d-161">These values are not real.</span></span> <span data-ttu-id="9593d-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9593d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9593d-163">Obraťte se na [tým podpory PerformanceCentre klienta](https://www.performancecentre.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="9593d-163">Contact [PerformanceCentre Client support team](https://www.performancecentre.com/contact-us/) to get these values.</span></span> 

4. <span data-ttu-id="9593d-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9593d-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. <span data-ttu-id="9593d-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9593d-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9593d-168">Na **PerformanceCentre konfigurace** klikněte na tlačítko **konfigurace PerformanceCentre** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9593d-168">On the **PerformanceCentre Configuration** section, click **Configure PerformanceCentre** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9593d-169">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9593d-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. <span data-ttu-id="9593d-171">Přihlášení k vaší **PerformanceCentre** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="9593d-171">Sign-on to your **PerformanceCentre** company site as administrator.</span></span>

8. <span data-ttu-id="9593d-172">Na kartě na levé straně klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="9593d-172">In the tab on the left side, click **Configure**.</span></span>
   
    ![Azure AD jednotné přihlášení][10]

9. <span data-ttu-id="9593d-174">Na kartě na levé straně klikněte na tlačítko **různé**a potom klikněte na **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9593d-174">In the tab on the left side, click **Miscellaneous**, and then click **Single Sign On**.</span></span>
   
    ![Azure AD jednotné přihlášení][11]

10. <span data-ttu-id="9593d-176">Jako **protokol**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="9593d-176">As **Protocol**, select **SAML**.</span></span>
   
    ![Azure AD jednotné přihlášení][12]

11. <span data-ttu-id="9593d-178">V poznámkovém bloku otevřete váš soubor stažený metadata, kopírovat obsah, vložte jej do **metadat zprostředkovatelů Identity** textovému poli a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9593d-178">Open your downloaded metadata file in notepad, copy the content, paste it into the **Identity Provider Metadata** textbox, and then click **Save**.</span></span>
   
    ![Azure AD jednotné přihlášení][13]

12. <span data-ttu-id="9593d-180">Ověřte, že hodnoty **Entity základní adresa URL** a **Entity ID URL** jsou správné.</span><span class="sxs-lookup"><span data-stu-id="9593d-180">Verify that the values for the **Entity Base URL** and **Entity ID URL** are correct.</span></span>
    
     ![Azure AD jednotné přihlášení][14]

> [!TIP]
> <span data-ttu-id="9593d-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9593d-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9593d-183">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9593d-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9593d-184">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9593d-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9593d-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9593d-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="9593d-186">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9593d-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9593d-188">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9593d-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9593d-189">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9593d-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9593d-191">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9593d-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9593d-193">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9593d-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9593d-195">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9593d-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9593d-197">a.</span><span class="sxs-lookup"><span data-stu-id="9593d-197">a.</span></span> <span data-ttu-id="9593d-198">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9593d-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9593d-199">b.</span><span class="sxs-lookup"><span data-stu-id="9593d-199">b.</span></span> <span data-ttu-id="9593d-200">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9593d-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9593d-201">c.</span><span class="sxs-lookup"><span data-stu-id="9593d-201">c.</span></span> <span data-ttu-id="9593d-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9593d-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9593d-203">d.</span><span class="sxs-lookup"><span data-stu-id="9593d-203">d.</span></span> <span data-ttu-id="9593d-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9593d-204">Click **Create**.</span></span>
 
### <a name="creating-a-performancecentre-test-user"></a><span data-ttu-id="9593d-205">Vytvoření zkušebního uživatele PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="9593d-205">Creating a PerformanceCentre test user</span></span>

<span data-ttu-id="9593d-206">Cílem této části je vytvoření uživatele v PerformanceCentre nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9593d-206">The objective of this section is to create a user called Britta Simon in PerformanceCentre.</span></span>

<span data-ttu-id="9593d-207">**Vytvoření uživatele v PerformanceCentre nazývá Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9593d-207">**To create a user called Britta Simon in PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="9593d-208">Přihlásit se k serveru vaší společnosti PerformanceCentre jako správce.</span><span class="sxs-lookup"><span data-stu-id="9593d-208">Sign on to your PerformanceCentre company site as administrator.</span></span>

2. <span data-ttu-id="9593d-209">V nabídce na levé straně klikněte na tlačítko **Interrelate**a potom klikněte na **vytvořit účastník**.</span><span class="sxs-lookup"><span data-stu-id="9593d-209">In the menu on the left, click **Interrelate**, and then click **Create Participant**.</span></span>
   
    ![Vytvoření uživatele][400]

3. <span data-ttu-id="9593d-211">Na **vztah mezi – vytvoření účastník** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9593d-211">On the **Interrelate - Create Participant** dialog, perform the following steps:</span></span>
   
    ![Vytvoření uživatele][401]
    
    <span data-ttu-id="9593d-213">a.</span><span class="sxs-lookup"><span data-stu-id="9593d-213">a.</span></span> <span data-ttu-id="9593d-214">Zadejte povinné atributy pro Britta Simon do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="9593d-214">Type the required attributes for Britta Simon into related textboxes.</span></span>
    
    >[!IMPORTANT]
    ><span data-ttu-id="9593d-215">Atribut Britta na uživatelské jméno v PerformanceCentre musí být stejné jako uživatelské jméno ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9593d-215">Britta's User Name attribute in PerformanceCentre must be the same as the User Name in Azure AD.</span></span>
    
    <span data-ttu-id="9593d-216">b.</span><span class="sxs-lookup"><span data-stu-id="9593d-216">b.</span></span> <span data-ttu-id="9593d-217">Vyberte **správce klienta** jako **zvolte roli**.</span><span class="sxs-lookup"><span data-stu-id="9593d-217">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="9593d-218">c.</span><span class="sxs-lookup"><span data-stu-id="9593d-218">c.</span></span> <span data-ttu-id="9593d-219">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9593d-219">Click **Save**.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9593d-220">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9593d-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9593d-221">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="9593d-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PerformanceCentre.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9593d-223">**Pokud chcete přiřadit Britta Simon PerformanceCentre, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9593d-223">**To assign Britta Simon to PerformanceCentre, perform the following steps:**</span></span>

1. <span data-ttu-id="9593d-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9593d-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9593d-226">V seznamu aplikací vyberte **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="9593d-226">In the applications list, select **PerformanceCentre**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. <span data-ttu-id="9593d-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9593d-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9593d-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9593d-230">Click **Add** button.</span></span> <span data-ttu-id="9593d-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9593d-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9593d-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9593d-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9593d-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9593d-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9593d-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9593d-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9593d-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9593d-236">Testing single sign-on</span></span>

<span data-ttu-id="9593d-237">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="9593d-237">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="9593d-238">Když kliknete na dlaždici PerformanceCentre na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="9593d-238">When you click the PerformanceCentre tile in the Access Panel, you should get automatically signed-on to your PerformanceCentre application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9593d-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9593d-239">Additional resources</span></span>

* [<span data-ttu-id="9593d-240">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9593d-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9593d-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9593d-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png

