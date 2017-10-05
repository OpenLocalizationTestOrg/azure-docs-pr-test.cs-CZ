---
title: "Kurz: Azure Active Directory integrace s platformou správy kontrakt Icertis | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a platformy pro správu Icertis kontrakt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6627e6dd-f559-4cd4-a509-f6d9a4961b49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 9dd002f71b7a960338071db869f7c8cf88071342
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a><span data-ttu-id="dfd76-103">Kurz: Azure Active Directory integrace s platformou správy Icertis kontraktu</span><span class="sxs-lookup"><span data-stu-id="dfd76-103">Tutorial: Azure Active Directory integration with Icertis Contract Management Platform</span></span>

<span data-ttu-id="dfd76-104">V tomto kurzu zjistíte Integrace platformy správy Icertis kontraktu služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dfd76-104">In this tutorial, you learn how to integrate Icertis Contract Management Platform with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dfd76-105">Integrace platformy správy kontrakt Icertis s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="dfd76-105">Integrating Icertis Contract Management Platform with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dfd76-106">Můžete řídit ve službě Azure AD, který má přístup k platformě správy Icertis kontraktu</span><span class="sxs-lookup"><span data-stu-id="dfd76-106">You can control in Azure AD who has access to Icertis Contract Management Platform</span></span>
- <span data-ttu-id="dfd76-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k platformě správy kontrakt Icertis (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfd76-107">You can enable your users to automatically get signed-on to Icertis Contract Management Platform (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dfd76-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dfd76-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dfd76-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dfd76-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dfd76-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dfd76-110">Prerequisites</span></span>

<span data-ttu-id="dfd76-111">Ke konfiguraci integrace služby Azure AD s platformou správy Icertis kontrakt, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="dfd76-111">To configure Azure AD integration with Icertis Contract Management Platform, you need the following items:</span></span>

- <span data-ttu-id="dfd76-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfd76-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dfd76-113">Platforma pro správu kontrakt Icertis jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="dfd76-113">An Icertis Contract Management Platform single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dfd76-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="dfd76-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dfd76-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="dfd76-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dfd76-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="dfd76-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dfd76-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dfd76-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dfd76-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="dfd76-118">Scenario description</span></span>
<span data-ttu-id="dfd76-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="dfd76-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dfd76-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="dfd76-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dfd76-121">Přidání platforma pro správu kontrakt Icertis z Galerie</span><span class="sxs-lookup"><span data-stu-id="dfd76-121">Adding Icertis Contract Management Platform from the gallery</span></span>
2. <span data-ttu-id="dfd76-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfd76-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icertis-contract-management-platform-from-the-gallery"></a><span data-ttu-id="dfd76-123">Přidání platforma pro správu kontrakt Icertis z Galerie</span><span class="sxs-lookup"><span data-stu-id="dfd76-123">Adding Icertis Contract Management Platform from the gallery</span></span>
<span data-ttu-id="dfd76-124">Chcete-li konfigurace integrace platformy správy kontrakt Icertis do služby Azure AD, přidejte platformu správy kontrakt Icertis z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="dfd76-124">To configure the integration of Icertis Contract Management Platform into Azure AD, you need to add Icertis Contract Management Platform from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dfd76-125">**Pokud chcete přidat platforma pro správu kontrakt Icertis z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="dfd76-125">**To add Icertis Contract Management Platform from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd76-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dfd76-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dfd76-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dfd76-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="dfd76-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfd76-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="dfd76-133">Do vyhledávacího pole zadejte **platforma pro správu kontrakt Icertis**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-133">In the search box, type **Icertis Contract Management Platform**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_search.png)

5. <span data-ttu-id="dfd76-135">Na panelu výsledků vyberte **platforma pro správu kontrakt Icertis**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dfd76-135">In the results panel, select **Icertis Contract Management Platform**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dfd76-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfd76-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dfd76-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s platformou správy kontrakt Icertis podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="dfd76-138">In this section, you configure and test Azure AD single sign-on with Icertis Contract Management Platform based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dfd76-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v platforma pro správu kontrakt Icertis je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dfd76-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Icertis Contract Management Platform is to a user in Azure AD.</span></span> <span data-ttu-id="dfd76-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské Icertis kontrakt správy platformy je potřeba navázat.</span><span class="sxs-lookup"><span data-stu-id="dfd76-140">In other words, a link relationship between an Azure AD user and the related user in Icertis Contract Management Platform needs to be established.</span></span>

<span data-ttu-id="dfd76-141">V Icertis platforma pro správu kontrakt, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="dfd76-141">In Icertis Contract Management Platform, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dfd76-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s platformou správy Icertis kontrakt, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="dfd76-142">To configure and test Azure AD single sign-on with Icertis Contract Management Platform, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dfd76-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="dfd76-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dfd76-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dfd76-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dfd76-145">**[Vytváření testovacího uživatele platforma pro správu kontrakt Icertis](#creating-an-icertis-contract-management-platform-test-user)**  – Pokud chcete mít protějšek Britta Simon v platforma pro správu kontrakt Icertis propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="dfd76-145">**[Creating an Icertis Contract Management Platform test user](#creating-an-icertis-contract-management-platform-test-user)** - to have a counterpart of Britta Simon in Icertis Contract Management Platform that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dfd76-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dfd76-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dfd76-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dfd76-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dfd76-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfd76-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dfd76-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci platformy správy Icertis kontrakt.</span><span class="sxs-lookup"><span data-stu-id="dfd76-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Icertis Contract Management Platform application.</span></span>

<span data-ttu-id="dfd76-150">**Ke konfiguraci Azure AD jednotné přihlašování s platformou správy Icertis kontrakt, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dfd76-150">**To configure Azure AD single sign-on with Icertis Contract Management Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd76-151">Na portálu Azure na **platforma pro správu kontrakt Icertis** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-151">In the Azure portal, on the **Icertis Contract Management Platform** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="dfd76-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dfd76-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_samlbase.png)

3. <span data-ttu-id="dfd76-155">Na **Icertis kontrakt správy platformy domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dfd76-155">On the **Icertis Contract Management Platform Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_url.png)

    <span data-ttu-id="dfd76-157">a.</span><span class="sxs-lookup"><span data-stu-id="dfd76-157">a.</span></span> <span data-ttu-id="dfd76-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="dfd76-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.icertis.com`</span></span>

    <span data-ttu-id="dfd76-159">b.</span><span class="sxs-lookup"><span data-stu-id="dfd76-159">b.</span></span> <span data-ttu-id="dfd76-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="dfd76-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.icertis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dfd76-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="dfd76-161">These values are not real.</span></span> <span data-ttu-id="dfd76-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="dfd76-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="dfd76-163">Obraťte se na [tým podpory Icertis kontrakt správy platformy v klientu](https://www.icertis.com/company/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="dfd76-163">Contact [Icertis Contract Management Platform Client support team](https://www.icertis.com/company/contact/) to get these values.</span></span> 

4. <span data-ttu-id="dfd76-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="dfd76-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_certificate.png) 

5. <span data-ttu-id="dfd76-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dfd76-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-icertisicm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dfd76-168">Na **konfigurace platformy správy kontrakt Icertis** klikněte na tlačítko **konfigurovat platformy pro správu kontrakt Icertis** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="dfd76-168">On the **Icertis Contract Management Platform Configuration** section, click **Configure Icertis Contract Management Platform** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dfd76-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="dfd76-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_configure.png) 

7. <span data-ttu-id="dfd76-171">Konfigurace jednotného přihlašování na **platforma pro správu kontrakt Icertis** straně, budete muset odeslat stažené **soubor XML s metadaty** a **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory platformy správy kontrakt Icertis](https://www.icertis.com/company/contact/).</span><span class="sxs-lookup"><span data-stu-id="dfd76-171">To configure single sign-on on **Icertis Contract Management Platform** side, you need to send the downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="dfd76-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="dfd76-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dfd76-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="dfd76-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dfd76-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dfd76-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dfd76-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfd76-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="dfd76-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dfd76-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="dfd76-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dfd76-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd76-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dfd76-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dfd76-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dfd76-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfd76-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dfd76-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dfd76-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dfd76-187">a.</span><span class="sxs-lookup"><span data-stu-id="dfd76-187">a.</span></span> <span data-ttu-id="dfd76-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dfd76-189">b.</span><span class="sxs-lookup"><span data-stu-id="dfd76-189">b.</span></span> <span data-ttu-id="dfd76-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dfd76-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dfd76-191">c.</span><span class="sxs-lookup"><span data-stu-id="dfd76-191">c.</span></span> <span data-ttu-id="dfd76-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dfd76-193">d.</span><span class="sxs-lookup"><span data-stu-id="dfd76-193">d.</span></span> <span data-ttu-id="dfd76-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-194">Click **Create**.</span></span>
 
### <a name="creating-an-icertis-contract-management-platform-test-user"></a><span data-ttu-id="dfd76-195">Vytváření testovacího uživatele platforma pro správu Icertis kontraktu</span><span class="sxs-lookup"><span data-stu-id="dfd76-195">Creating an Icertis Contract Management Platform test user</span></span>

<span data-ttu-id="dfd76-196">V této části vytvoříte uživatele volal Britta Simon v platforma pro správu Icertis kontrakt.</span><span class="sxs-lookup"><span data-stu-id="dfd76-196">In this section, you create a user called Britta Simon in Icertis Contract Management Platform.</span></span> <span data-ttu-id="dfd76-197">Spojte se s [tým podpory platformy správy kontrakt Icertis](https://www.icertis.com/company/contact/) přidat uživatele do platformy pro správu Icertis kontrakt.</span><span class="sxs-lookup"><span data-stu-id="dfd76-197">Please work with [Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/) to add the users in the Icertis Contract Management Platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dfd76-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dfd76-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dfd76-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k platformě správy Icertis kontrakt.</span><span class="sxs-lookup"><span data-stu-id="dfd76-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Icertis Contract Management Platform.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="dfd76-201">**Pokud chcete přiřadit Britta Simon platforma pro správu Icertis kontrakt, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dfd76-201">**To assign Britta Simon to Icertis Contract Management Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd76-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="dfd76-204">V seznamu aplikací vyberte **platforma pro správu kontrakt Icertis**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-204">In the applications list, select **Icertis Contract Management Platform**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_app.png) 

3. <span data-ttu-id="dfd76-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dfd76-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="dfd76-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dfd76-208">Click **Add** button.</span></span> <span data-ttu-id="dfd76-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfd76-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="dfd76-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dfd76-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dfd76-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfd76-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dfd76-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dfd76-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dfd76-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dfd76-214">Testing single sign-on</span></span>

<span data-ttu-id="dfd76-215">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="dfd76-215">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="dfd76-216">Když kliknete na dlaždici platforma pro správu kontrakt Icertis na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci platformy správy Icertis kontrakt.</span><span class="sxs-lookup"><span data-stu-id="dfd76-216">When you click the Icertis Contract Management Platform tile in the Access Panel, you should get automatically signed-on to your Icertis Contract Management Platform application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dfd76-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dfd76-217">Additional resources</span></span>

* [<span data-ttu-id="dfd76-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dfd76-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dfd76-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dfd76-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_203.png

