---
title: 'Kurz: Azure Active Directory integrace s Weekdone | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Weekdone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 84aa0069dce55a6623398a99e1cac6bb21bf52f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a><span data-ttu-id="5dfb3-103">Kurz: Azure Active Directory integrace s Weekdone</span><span class="sxs-lookup"><span data-stu-id="5dfb3-103">Tutorial: Azure Active Directory integration with Weekdone</span></span>

<span data-ttu-id="5dfb3-104">V tomto kurzu zjistěte, jak integrovat Weekdone s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5dfb3-104">In this tutorial, you learn how to integrate Weekdone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5dfb3-105">Integrace Weekdone s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-105">Integrating Weekdone with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5dfb3-106">Můžete řídit ve službě Azure AD, který má přístup k Weekdone</span><span class="sxs-lookup"><span data-stu-id="5dfb3-106">You can control in Azure AD who has access to Weekdone</span></span>
- <span data-ttu-id="5dfb3-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Weekdone (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5dfb3-107">You can enable your users to automatically get signed-on to Weekdone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5dfb3-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5dfb3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5dfb3-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5dfb3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5dfb3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5dfb3-110">Prerequisites</span></span>

<span data-ttu-id="5dfb3-111">Konfigurace integrace Azure AD s Weekdone, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-111">To configure Azure AD integration with Weekdone, you need the following items:</span></span>

- <span data-ttu-id="5dfb3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5dfb3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5dfb3-113">Weekdone jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5dfb3-113">A Weekdone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5dfb3-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5dfb3-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5dfb3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5dfb3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5dfb3-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5dfb3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5dfb3-118">Scenario description</span></span>
<span data-ttu-id="5dfb3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5dfb3-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5dfb3-121">Přidání Weekdone z Galerie</span><span class="sxs-lookup"><span data-stu-id="5dfb3-121">Adding Weekdone from the gallery</span></span>
2. <span data-ttu-id="5dfb3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5dfb3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-weekdone-from-the-gallery"></a><span data-ttu-id="5dfb3-123">Přidání Weekdone z Galerie</span><span class="sxs-lookup"><span data-stu-id="5dfb3-123">Adding Weekdone from the gallery</span></span>
<span data-ttu-id="5dfb3-124">Při konfiguraci integrace Weekdone do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Weekdone z galerie.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-124">To configure the integration of Weekdone into Azure AD, you need to add Weekdone from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5dfb3-125">**Pokud chcete přidat Weekdone z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5dfb3-125">**To add Weekdone from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5dfb3-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5dfb3-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5dfb3-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5dfb3-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5dfb3-133">Do vyhledávacího pole zadejte **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-133">In the search box, type **Weekdone**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_search.png)

5. <span data-ttu-id="5dfb3-135">Na panelu výsledků vyberte **Weekdone**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-135">In the results panel, select **Weekdone**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5dfb3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5dfb3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5dfb3-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Weekdone podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5dfb3-138">In this section, you configure and test Azure AD single sign-on with Weekdone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5dfb3-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Weekdone je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Weekdone is to a user in Azure AD.</span></span> <span data-ttu-id="5dfb3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Weekdone musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-140">In other words, a link relationship between an Azure AD user and the related user in Weekdone needs to be established.</span></span>

<span data-ttu-id="5dfb3-141">V Weekdone, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-141">In Weekdone, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5dfb3-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Weekdone, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-142">To configure and test Azure AD single sign-on with Weekdone, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5dfb3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5dfb3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5dfb3-145">**[Vytvoření zkušebního uživatele Weekdone](#creating-a-weekdone-test-user)**  – Pokud chcete mít protějšek Britta Simon v Weekdone propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-145">**[Creating a Weekdone test user](#creating-a-weekdone-test-user)** - to have a counterpart of Britta Simon in Weekdone that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5dfb3-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5dfb3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5dfb3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5dfb3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5dfb3-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Weekdone.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Weekdone application.</span></span>

<span data-ttu-id="5dfb3-150">**Ke konfiguraci Azure AD jednotné přihlašování s Weekdone, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5dfb3-150">**To configure Azure AD single sign-on with Weekdone, perform the following steps:**</span></span>

1. <span data-ttu-id="5dfb3-151">Na portálu Azure na **Weekdone** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-151">In the Azure portal, on the **Weekdone** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5dfb3-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_samlbase.png)

3. <span data-ttu-id="5dfb3-155">Na **Weekdone domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-155">On the **Weekdone Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url1.png)

    <span data-ttu-id="5dfb3-157">a.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-157">a.</span></span> <span data-ttu-id="5dfb3-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="5dfb3-158">In the **Identifier** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

    <span data-ttu-id="5dfb3-159">b.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-159">b.</span></span> <span data-ttu-id="5dfb3-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="5dfb3-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

4. <span data-ttu-id="5dfb3-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="5dfb3-162">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url2.png)

    <span data-ttu-id="5dfb3-164">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="5dfb3-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="5dfb3-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-165">These values are not real.</span></span> <span data-ttu-id="5dfb3-166">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="5dfb3-167">Obraťte se na [tým podpory Weekdone klienta](mailto:hello@weekdone.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-167">Contact [Weekdone Client support team](mailto:hello@weekdone.com) to get these values.</span></span> 

5. <span data-ttu-id="5dfb3-168">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-168">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_certificate.png) 

6. <span data-ttu-id="5dfb3-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="5dfb3-172">Na **Weekdone konfigurace** klikněte na tlačítko **konfigurace Weekdone** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-172">On the **Weekdone Configuration** section, click **Configure Weekdone** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5dfb3-173">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5dfb3-173">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_configure.png) 

8. <span data-ttu-id="5dfb3-175">Konfigurace jednotného přihlašování na **Weekdone** straně, budete muset odeslat stažené **soubor XML s metadaty, adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [Weekdone tým podpory ](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="5dfb3-175">To configure single sign-on on **Weekdone** side, you need to send the downloaded **Metadata XML, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Weekdone support team](mailto:hello@weekdone.com).</span></span>

> [!TIP]
> <span data-ttu-id="5dfb3-176">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5dfb3-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5dfb3-177">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5dfb3-178">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5dfb3-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5dfb3-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5dfb3-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="5dfb3-180">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5dfb3-182">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5dfb3-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5dfb3-183">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5dfb3-185">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5dfb3-187">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5dfb3-189">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5dfb3-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5dfb3-191">a.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-191">a.</span></span> <span data-ttu-id="5dfb3-192">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5dfb3-193">b.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-193">b.</span></span> <span data-ttu-id="5dfb3-194">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5dfb3-195">c.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-195">c.</span></span> <span data-ttu-id="5dfb3-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5dfb3-197">d.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-197">d.</span></span> <span data-ttu-id="5dfb3-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-198">Click **Create**.</span></span>
 
### <a name="creating-a-weekdone-test-user"></a><span data-ttu-id="5dfb3-199">Vytvoření zkušebního uživatele Weekdone</span><span class="sxs-lookup"><span data-stu-id="5dfb3-199">Creating a Weekdone test user</span></span>

<span data-ttu-id="5dfb3-200">Cílem této části je vytvoření uživatele v Weekdone nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-200">The objective of this section is to create a user called Britta Simon in Weekdone.</span></span> <span data-ttu-id="5dfb3-201">Weekdone podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-201">Weekdone supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="5dfb3-202">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-202">There is no action item for you in this section.</span></span> <span data-ttu-id="5dfb3-203">Nový uživatel se vytvoří během pokusu o přístup k Weekdone, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-203">A new user is created during an attempt to access Weekdone if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="5dfb3-204">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Weekdone klienta](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="5dfb3-204">If you need to create a user manually, you need to contact the [Weekdone Client support team](mailto:hello@weekdone.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5dfb3-205">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5dfb3-205">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5dfb3-206">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Weekdone.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-206">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Weekdone.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5dfb3-208">**Pokud chcete přiřadit Britta Simon Weekdone, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5dfb3-208">**To assign Britta Simon to Weekdone, perform the following steps:**</span></span>

1. <span data-ttu-id="5dfb3-209">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-209">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5dfb3-211">V seznamu aplikací vyberte **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-211">In the applications list, select **Weekdone**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_app.png) 

3. <span data-ttu-id="5dfb3-213">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-213">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5dfb3-215">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-215">Click **Add** button.</span></span> <span data-ttu-id="5dfb3-216">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-216">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5dfb3-218">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-218">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5dfb3-219">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-219">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5dfb3-220">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-220">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5dfb3-221">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5dfb3-221">Testing single sign-on</span></span>

<span data-ttu-id="5dfb3-222">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-222">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="5dfb3-223">Když kliknete na dlaždici Weekdone na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Weekdone.</span><span class="sxs-lookup"><span data-stu-id="5dfb3-223">When you click the Weekdone tile in the Access Panel, you should get automatically signed-on to your Weekdone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5dfb3-224">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5dfb3-224">Additional resources</span></span>

* [<span data-ttu-id="5dfb3-225">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5dfb3-225">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5dfb3-226">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5dfb3-226">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_203.png

