---
title: 'Kurz: Azure Active Directory integrace s UltiPro | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a UltiPro."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: afc0f2b9-2eac-47ec-af04-65ed0fb0ca5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: ab60bda1be7101d5bd0c51f5499a820db40375bf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ultipro"></a><span data-ttu-id="a935f-103">Kurz: Azure Active Directory integrace s UltiPro</span><span class="sxs-lookup"><span data-stu-id="a935f-103">Tutorial: Azure Active Directory integration with UltiPro</span></span>

<span data-ttu-id="a935f-104">V tomto kurzu zjistěte, jak integrovat UltiPro s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a935f-104">In this tutorial, you learn how to integrate UltiPro with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a935f-105">Integrace UltiPro s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a935f-105">Integrating UltiPro with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a935f-106">Můžete řídit ve službě Azure AD, který má přístup k UltiPro</span><span class="sxs-lookup"><span data-stu-id="a935f-106">You can control in Azure AD who has access to UltiPro</span></span>
- <span data-ttu-id="a935f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k UltiPro (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a935f-107">You can enable your users to automatically get signed-on to UltiPro (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a935f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a935f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a935f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a935f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a935f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a935f-110">Prerequisites</span></span>

<span data-ttu-id="a935f-111">Konfigurace integrace Azure AD s UltiPro, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a935f-111">To configure Azure AD integration with UltiPro, you need the following items:</span></span>

- <span data-ttu-id="a935f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a935f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a935f-113">UltiPro jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a935f-113">A UltiPro single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a935f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a935f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a935f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a935f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a935f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a935f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a935f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a935f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a935f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a935f-118">Scenario description</span></span>
<span data-ttu-id="a935f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a935f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a935f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a935f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a935f-121">Přidání UltiPro z Galerie</span><span class="sxs-lookup"><span data-stu-id="a935f-121">Adding UltiPro from the gallery</span></span>
2. <span data-ttu-id="a935f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a935f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ultipro-from-the-gallery"></a><span data-ttu-id="a935f-123">Přidání UltiPro z Galerie</span><span class="sxs-lookup"><span data-stu-id="a935f-123">Adding UltiPro from the gallery</span></span>
<span data-ttu-id="a935f-124">Při konfiguraci integrace UltiPro do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS UltiPro z galerie.</span><span class="sxs-lookup"><span data-stu-id="a935f-124">To configure the integration of UltiPro into Azure AD, you need to add UltiPro from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a935f-125">**Pokud chcete přidat UltiPro z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a935f-125">**To add UltiPro from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a935f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a935f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a935f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a935f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a935f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a935f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a935f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a935f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a935f-133">Do vyhledávacího pole zadejte **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="a935f-133">In the search box, type **UltiPro**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_search.png)

5. <span data-ttu-id="a935f-135">Na panelu výsledků vyberte **UltiPro**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a935f-135">In the results panel, select **UltiPro**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a935f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a935f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a935f-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UltiPro podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a935f-138">In this section, you configure and test Azure AD single sign-on with UltiPro based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a935f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v UltiPro je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a935f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in UltiPro is to a user in Azure AD.</span></span> <span data-ttu-id="a935f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v UltiPro musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a935f-140">In other words, a link relationship between an Azure AD user and the related user in UltiPro needs to be established.</span></span>

<span data-ttu-id="a935f-141">V UltiPro, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a935f-141">In UltiPro, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a935f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s UltiPro, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a935f-142">To configure and test Azure AD single sign-on with UltiPro, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a935f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a935f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a935f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a935f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a935f-145">**[Vytvoření zkušebního uživatele UltiPro](#creating-a-ultipro-test-user)**  – Pokud chcete mít protějšek Britta Simon v UltiPro propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a935f-145">**[Creating a UltiPro test user](#creating-a-ultipro-test-user)** - to have a counterpart of Britta Simon in UltiPro that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a935f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a935f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a935f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a935f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a935f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a935f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a935f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci UltiPro.</span><span class="sxs-lookup"><span data-stu-id="a935f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UltiPro application.</span></span>

<span data-ttu-id="a935f-150">**Ke konfiguraci Azure AD jednotné přihlašování s UltiPro, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a935f-150">**To configure Azure AD single sign-on with UltiPro, perform the following steps:**</span></span>

1. <span data-ttu-id="a935f-151">Na portálu Azure na **UltiPro** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a935f-151">In the Azure portal, on the **UltiPro** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a935f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a935f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_samlbase.png)

3. <span data-ttu-id="a935f-155">Na **UltiPro domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a935f-155">On the **UltiPro Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_url.png)

    <span data-ttu-id="a935f-157">a.</span><span class="sxs-lookup"><span data-stu-id="a935f-157">a.</span></span> <span data-ttu-id="a935f-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a935f-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/`|
    | `https://<companyname>.ultiproworkplace.com?cpi=AZUREADISSSUERURL`|
    | ` https://<companyname>.ultipro.ca`|
    
    <span data-ttu-id="a935f-159">b.</span><span class="sxs-lookup"><span data-stu-id="a935f-159">b.</span></span> <span data-ttu-id="a935f-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a935f-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/adfs/services/trust`|
    | `https://<companyname>.ultiproworkplace.com/adfs/services/trust`|
    | `https://<companyname>.ultipro.ca/adfs/services/trust`|
    
    <span data-ttu-id="a935f-161">c.</span><span class="sxs-lookup"><span data-stu-id="a935f-161">c.</span></span> <span data-ttu-id="a935f-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a935f-162">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/<instancename>`|
    | `https://<companyname>.ultiproworkplace.com/<instancename>`|
    | `https://<companyname>.ultipro.ca/<instancename>`|
    
    > [!NOTE] 
    > <span data-ttu-id="a935f-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a935f-163">These values are not real.</span></span> <span data-ttu-id="a935f-164">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="a935f-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="a935f-165">Obraťte se na [tým podpory UltiPro klienta](https://www.ultimatesoftware.com/ContactUs) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a935f-165">Contact [UltiPro Client support team](https://www.ultimatesoftware.com/ContactUs) to get these values.</span></span> 

5. <span data-ttu-id="a935f-166">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a935f-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_certificate.png) 

6. <span data-ttu-id="a935f-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a935f-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="a935f-170">Na **UltiPro konfigurace** klikněte na tlačítko **konfigurace UltiPro** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a935f-170">On the **UltiPro Configuration** section, click **Configure UltiPro** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a935f-171">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a935f-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_configure.png) 

8. <span data-ttu-id="a935f-173">Konfigurace jednotného přihlašování na **UltiPro** straně, budete muset odeslat stažené **Certificate(Base64), adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [UltiPro podpory tým](https://www.ultimatesoftware.com/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="a935f-173">To configure single sign-on on **UltiPro** side, you need to send the downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [UltiPro support team](https://www.ultimatesoftware.com/ContactUs).</span></span>

> [!TIP]
> <span data-ttu-id="a935f-174">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a935f-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a935f-175">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a935f-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a935f-176">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a935f-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a935f-177">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a935f-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="a935f-178">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a935f-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a935f-180">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a935f-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a935f-181">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a935f-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a935f-183">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a935f-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a935f-185">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a935f-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a935f-187">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a935f-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a935f-189">a.</span><span class="sxs-lookup"><span data-stu-id="a935f-189">a.</span></span> <span data-ttu-id="a935f-190">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a935f-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a935f-191">b.</span><span class="sxs-lookup"><span data-stu-id="a935f-191">b.</span></span> <span data-ttu-id="a935f-192">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a935f-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a935f-193">c.</span><span class="sxs-lookup"><span data-stu-id="a935f-193">c.</span></span> <span data-ttu-id="a935f-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a935f-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a935f-195">d.</span><span class="sxs-lookup"><span data-stu-id="a935f-195">d.</span></span> <span data-ttu-id="a935f-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a935f-196">Click **Create**.</span></span>
 
### <a name="creating-a-ultipro-test-user"></a><span data-ttu-id="a935f-197">Vytvoření zkušebního uživatele UltiPro</span><span class="sxs-lookup"><span data-stu-id="a935f-197">Creating a UltiPro test user</span></span>

<span data-ttu-id="a935f-198">V této části vytvoříte volal Britta Simon v UltiPro uživatele.</span><span class="sxs-lookup"><span data-stu-id="a935f-198">In this section, you create a user called Britta Simon in UltiPro.</span></span> <span data-ttu-id="a935f-199">Práce s [tým podpory UltiPro](https://www.ultimatesoftware.com/ContactUs) přidat uživatele do UltiPro platformy.</span><span class="sxs-lookup"><span data-stu-id="a935f-199">Work with [UltiPro support team](https://www.ultimatesoftware.com/ContactUs) to add the users in the UltiPro platform.</span></span> <span data-ttu-id="a935f-200">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a935f-200">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a935f-201">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a935f-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a935f-202">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu UltiPro.</span><span class="sxs-lookup"><span data-stu-id="a935f-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UltiPro.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a935f-204">**Pokud chcete přiřadit Britta Simon UltiPro, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a935f-204">**To assign Britta Simon to UltiPro, perform the following steps:**</span></span>

1. <span data-ttu-id="a935f-205">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a935f-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a935f-207">V seznamu aplikací vyberte **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="a935f-207">In the applications list, select **UltiPro**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_app.png) 

3. <span data-ttu-id="a935f-209">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a935f-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a935f-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a935f-211">Click **Add** button.</span></span> <span data-ttu-id="a935f-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a935f-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a935f-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a935f-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a935f-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a935f-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a935f-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a935f-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a935f-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a935f-217">Testing single sign-on</span></span>

<span data-ttu-id="a935f-218">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a935f-218">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="a935f-219">Když kliknete na dlaždici UltiPro na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci UltiPro.</span><span class="sxs-lookup"><span data-stu-id="a935f-219">When you click the UltiPro tile in the Access Panel, you should get automatically signed-on to your UltiPro application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a935f-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a935f-220">Additional resources</span></span>

* [<span data-ttu-id="a935f-221">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a935f-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a935f-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a935f-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_203.png

