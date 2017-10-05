---
title: 'Kurz: Azure Active Directory integrace s Tidemark | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Tidemark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5cf80d4e-6e8b-48ec-81c8-27872af5e5d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 170dc58363b12ec671c2fab8c80c7720d3dbf352
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tidemark"></a><span data-ttu-id="be431-103">Kurz: Azure Active Directory integrace s Tidemark</span><span class="sxs-lookup"><span data-stu-id="be431-103">Tutorial: Azure Active Directory integration with Tidemark</span></span>

<span data-ttu-id="be431-104">V tomto kurzu zjistěte, jak integrovat Tidemark s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="be431-104">In this tutorial, you learn how to integrate Tidemark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="be431-105">Integrace Tidemark s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="be431-105">Integrating Tidemark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="be431-106">Můžete řídit ve službě Azure AD, který má přístup k Tidemark</span><span class="sxs-lookup"><span data-stu-id="be431-106">You can control in Azure AD who has access to Tidemark</span></span>
- <span data-ttu-id="be431-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Tidemark (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="be431-107">You can enable your users to automatically get signed-on to Tidemark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="be431-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="be431-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="be431-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="be431-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be431-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="be431-110">Prerequisites</span></span>

<span data-ttu-id="be431-111">Konfigurace integrace Azure AD s Tidemark, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="be431-111">To configure Azure AD integration with Tidemark, you need the following items:</span></span>

- <span data-ttu-id="be431-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="be431-112">An Azure AD subscription</span></span>
- <span data-ttu-id="be431-113">Tidemark jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="be431-113">A Tidemark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="be431-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="be431-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="be431-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="be431-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="be431-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="be431-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="be431-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="be431-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="be431-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="be431-118">Scenario description</span></span>
<span data-ttu-id="be431-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="be431-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="be431-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="be431-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="be431-121">Přidání Tidemark z Galerie</span><span class="sxs-lookup"><span data-stu-id="be431-121">Adding Tidemark from the gallery</span></span>
2. <span data-ttu-id="be431-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="be431-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tidemark-from-the-gallery"></a><span data-ttu-id="be431-123">Přidání Tidemark z Galerie</span><span class="sxs-lookup"><span data-stu-id="be431-123">Adding Tidemark from the gallery</span></span>
<span data-ttu-id="be431-124">Při konfiguraci integrace Tidemark do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Tidemark z galerie.</span><span class="sxs-lookup"><span data-stu-id="be431-124">To configure the integration of Tidemark into Azure AD, you need to add Tidemark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="be431-125">**Pokud chcete přidat Tidemark z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="be431-125">**To add Tidemark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="be431-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="be431-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="be431-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="be431-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="be431-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="be431-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="be431-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="be431-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="be431-133">Do vyhledávacího pole zadejte **Tidemark**.</span><span class="sxs-lookup"><span data-stu-id="be431-133">In the search box, type **Tidemark**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_search.png)

5. <span data-ttu-id="be431-135">Na panelu výsledků vyberte **Tidemark**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="be431-135">In the results panel, select **Tidemark**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="be431-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="be431-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="be431-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tidemark podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="be431-138">In this section, you configure and test Azure AD single sign-on with Tidemark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="be431-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Tidemark je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="be431-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tidemark is to a user in Azure AD.</span></span> <span data-ttu-id="be431-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Tidemark musí navázat.</span><span class="sxs-lookup"><span data-stu-id="be431-140">In other words, a link relationship between an Azure AD user and the related user in Tidemark needs to be established.</span></span>

<span data-ttu-id="be431-141">V Tidemark, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="be431-141">In Tidemark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="be431-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tidemark, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="be431-142">To configure and test Azure AD single sign-on with Tidemark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="be431-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="be431-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="be431-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be431-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="be431-145">**[Vytvoření zkušebního uživatele Tidemark](#creating-a-tidemark-test-user)**  – Pokud chcete mít protějšek Britta Simon v Tidemark propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="be431-145">**[Creating a Tidemark test user](#creating-a-tidemark-test-user)** - to have a counterpart of Britta Simon in Tidemark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="be431-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="be431-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="be431-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="be431-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="be431-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="be431-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="be431-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Tidemark.</span><span class="sxs-lookup"><span data-stu-id="be431-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tidemark application.</span></span>

<span data-ttu-id="be431-150">**Ke konfiguraci Azure AD jednotné přihlašování s Tidemark, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="be431-150">**To configure Azure AD single sign-on with Tidemark, perform the following steps:**</span></span>

1. <span data-ttu-id="be431-151">Na portálu Azure na **Tidemark** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="be431-151">In the Azure portal, on the **Tidemark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="be431-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="be431-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_samlbase.png)

3. <span data-ttu-id="be431-155">Na **Tidemark domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="be431-155">On the **Tidemark Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_url.png)

    <span data-ttu-id="be431-157">a.</span><span class="sxs-lookup"><span data-stu-id="be431-157">a.</span></span> <span data-ttu-id="be431-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="be431-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    | |
    |--|
    | `https://<subdomain>.tidemark.com/login` |
    | `https://<subdomain>.tidemark.net/login` |

    <span data-ttu-id="be431-159">b.</span><span class="sxs-lookup"><span data-stu-id="be431-159">b.</span></span> <span data-ttu-id="be431-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="be431-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
    | |
    |--|
    | `https://<subdomain>.tidemark.com/saml` |
    | `https://<subdomain>.tidemark.net/saml` |

    > [!NOTE] 
    > <span data-ttu-id="be431-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="be431-161">These values are not real.</span></span> <span data-ttu-id="be431-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="be431-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="be431-163">Obraťte se na [tým podpory Tidemark klienta](http://www.tidemark.com/contact-us) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="be431-163">Contact [Tidemark Client support team](http://www.tidemark.com/contact-us) to get these values.</span></span> 
 
4. <span data-ttu-id="be431-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="be431-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_certificate.png) 

5. <span data-ttu-id="be431-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="be431-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tidemark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="be431-168">Na **Tidemark konfigurace** klikněte na tlačítko **konfigurace Tidemark** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="be431-168">On the **Tidemark Configuration** section, click **Configure Tidemark** to open **Configure sign-on** window.</span></span> <span data-ttu-id="be431-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="be431-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_configure.png) 

7. <span data-ttu-id="be431-171">Konfigurace jednotného přihlašování na **Tidemark** straně, budete muset odeslat stažené **Certificate(Base64), SAML Entity ID, Sign-Out adresu URL a SAML jeden přihlašování služby URL** k [tým podpory Tidemark](http://www.tidemark.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="be431-171">To configure single sign-on on **Tidemark** side, you need to send the downloaded **Certificate(Base64), SAML Entity ID, Sign-Out URL and SAML Single Sign-On Service URL** to [Tidemark support team](http://www.tidemark.com/contact-us).</span></span> <span data-ttu-id="be431-172">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="be431-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="be431-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="be431-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="be431-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="be431-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="be431-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="be431-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="be431-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="be431-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="be431-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be431-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="be431-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="be431-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="be431-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="be431-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="be431-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="be431-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="be431-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="be431-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="be431-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="be431-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="be431-188">a.</span><span class="sxs-lookup"><span data-stu-id="be431-188">a.</span></span> <span data-ttu-id="be431-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="be431-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="be431-190">b.</span><span class="sxs-lookup"><span data-stu-id="be431-190">b.</span></span> <span data-ttu-id="be431-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="be431-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="be431-192">c.</span><span class="sxs-lookup"><span data-stu-id="be431-192">c.</span></span> <span data-ttu-id="be431-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="be431-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="be431-194">d.</span><span class="sxs-lookup"><span data-stu-id="be431-194">d.</span></span> <span data-ttu-id="be431-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="be431-195">Click **Create**.</span></span>
 
### <a name="creating-a-tidemark-test-user"></a><span data-ttu-id="be431-196">Vytvoření zkušebního uživatele Tidemark</span><span class="sxs-lookup"><span data-stu-id="be431-196">Creating a Tidemark test user</span></span>

<span data-ttu-id="be431-197">Cílem této části je vytvoření uživatele v Tidemark nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be431-197">The objective of this section is to create a user called Britta Simon in Tidemark.</span></span> <span data-ttu-id="be431-198">Spojte se s [tým podpory Tidemark](http://www.tidemark.com/contact-us) přidat uživatele do Tidemark účtu.</span><span class="sxs-lookup"><span data-stu-id="be431-198">Please work with [Tidemark support team](http://www.tidemark.com/contact-us) to add the users in the Tidemark account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="be431-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="be431-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="be431-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Tidemark.</span><span class="sxs-lookup"><span data-stu-id="be431-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tidemark.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="be431-202">**Pokud chcete přiřadit Britta Simon Tidemark, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="be431-202">**To assign Britta Simon to Tidemark, perform the following steps:**</span></span>

1. <span data-ttu-id="be431-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="be431-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="be431-205">V seznamu aplikací vyberte **Tidemark**.</span><span class="sxs-lookup"><span data-stu-id="be431-205">In the applications list, select **Tidemark**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_app.png) 

3. <span data-ttu-id="be431-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="be431-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="be431-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="be431-209">Click **Add** button.</span></span> <span data-ttu-id="be431-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="be431-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="be431-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="be431-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="be431-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="be431-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="be431-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="be431-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="be431-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="be431-215">Testing single sign-on</span></span>

<span data-ttu-id="be431-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="be431-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="be431-217">Když kliknete na dlaždici Tidemark na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Tidemark.</span><span class="sxs-lookup"><span data-stu-id="be431-217">When you click the Tidemark tile in the Access Panel, you should get automatically signed-on to your Tidemark application.</span></span>
<span data-ttu-id="be431-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="be431-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be431-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="be431-219">Additional resources</span></span>

* [<span data-ttu-id="be431-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="be431-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="be431-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="be431-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_203.png

