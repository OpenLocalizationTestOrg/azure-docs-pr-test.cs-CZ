---
title: 'Kurz: Azure Active Directory integrace s Novatus | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Novatus."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2f13779-bdb7-4408-9738-be67ed3de4e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: jeedes
ms.openlocfilehash: ec67e96309a8877e6fb65b30da1501e4f34a9ee4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-novatus"></a><span data-ttu-id="74512-103">Kurz: Azure Active Directory integrace s Novatus</span><span class="sxs-lookup"><span data-stu-id="74512-103">Tutorial: Azure Active Directory integration with Novatus</span></span>

<span data-ttu-id="74512-104">V tomto kurzu zjistěte, jak integrovat Novatus s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="74512-104">In this tutorial, you learn how to integrate Novatus with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="74512-105">Integrace Novatus s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="74512-105">Integrating Novatus with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="74512-106">Můžete řídit ve službě Azure AD, který má přístup k Novatus</span><span class="sxs-lookup"><span data-stu-id="74512-106">You can control in Azure AD who has access to Novatus</span></span>
- <span data-ttu-id="74512-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Novatus (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="74512-107">You can enable your users to automatically get signed-on to Novatus (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="74512-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="74512-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="74512-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="74512-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74512-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74512-110">Prerequisites</span></span>

<span data-ttu-id="74512-111">Konfigurace integrace Azure AD s Novatus, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="74512-111">To configure Azure AD integration with Novatus, you need the following items:</span></span>

- <span data-ttu-id="74512-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="74512-112">An Azure AD subscription</span></span>
- <span data-ttu-id="74512-113">Novatus jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="74512-113">A Novatus single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="74512-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="74512-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="74512-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="74512-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="74512-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="74512-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="74512-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="74512-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="74512-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="74512-118">Scenario description</span></span>
<span data-ttu-id="74512-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="74512-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="74512-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="74512-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="74512-121">Přidání Novatus z Galerie</span><span class="sxs-lookup"><span data-stu-id="74512-121">Adding Novatus from the gallery</span></span>
2. <span data-ttu-id="74512-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="74512-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-novatus-from-the-gallery"></a><span data-ttu-id="74512-123">Přidání Novatus z Galerie</span><span class="sxs-lookup"><span data-stu-id="74512-123">Adding Novatus from the gallery</span></span>
<span data-ttu-id="74512-124">Při konfiguraci integrace Novatus do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Novatus z galerie.</span><span class="sxs-lookup"><span data-stu-id="74512-124">To configure the integration of Novatus into Azure AD, you need to add Novatus from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="74512-125">**Pokud chcete přidat Novatus z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74512-125">**To add Novatus from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="74512-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="74512-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="74512-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="74512-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="74512-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="74512-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="74512-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74512-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="74512-133">Do vyhledávacího pole zadejte **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="74512-133">In the search box, type **Novatus**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_search.png)

5. <span data-ttu-id="74512-135">Na panelu výsledků vyberte **Novatus**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74512-135">In the results panel, select **Novatus**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="74512-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="74512-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="74512-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Novatus podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="74512-138">In this section, you configure and test Azure AD single sign-on with Novatus based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="74512-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Novatus je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74512-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Novatus is to a user in Azure AD.</span></span> <span data-ttu-id="74512-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Novatus musí navázat.</span><span class="sxs-lookup"><span data-stu-id="74512-140">In other words, a link relationship between an Azure AD user and the related user in Novatus needs to be established.</span></span>

<span data-ttu-id="74512-141">V Novatus, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="74512-141">In Novatus, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="74512-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Novatus, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="74512-142">To configure and test Azure AD single sign-on with Novatus, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="74512-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="74512-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="74512-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74512-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="74512-145">**[Vytvoření zkušebního uživatele Novatus](#creating-a-novatus-test-user)**  – Pokud chcete mít protějšek Britta Simon v Novatus propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="74512-145">**[Creating a Novatus test user](#creating-a-novatus-test-user)** - to have a counterpart of Britta Simon in Novatus that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="74512-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="74512-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="74512-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="74512-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="74512-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="74512-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="74512-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Novatus.</span><span class="sxs-lookup"><span data-stu-id="74512-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Novatus application.</span></span>

<span data-ttu-id="74512-150">**Ke konfiguraci Azure AD jednotné přihlašování s Novatus, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74512-150">**To configure Azure AD single sign-on with Novatus, perform the following steps:**</span></span>

1. <span data-ttu-id="74512-151">Na portálu Azure na **Novatus** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="74512-151">In the Azure portal, on the **Novatus** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="74512-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="74512-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_samlbase.png)

3. <span data-ttu-id="74512-155">Na **Novatus domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="74512-155">On the **Novatus Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_url.png)

     <span data-ttu-id="74512-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://sso.novatuscontracts.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="74512-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.novatuscontracts.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="74512-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="74512-158">This value is not real.</span></span> <span data-ttu-id="74512-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="74512-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="74512-160">Obraťte se na [tým podpory Novatus klienta](mailto:jvinci@novatusinc.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="74512-160">Contact [Novatus Client support team](mailto:jvinci@novatusinc.com) to get this value.</span></span> 
 


4. <span data-ttu-id="74512-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="74512-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_certificate.png) 

5. <span data-ttu-id="74512-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74512-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-novatus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="74512-165">Na **Novatus konfigurace** klikněte na tlačítko **konfigurace Novatus** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="74512-165">On the **Novatus Configuration** section, click **Configure Novatus** to open **Configure sign-on** window.</span></span> <span data-ttu-id="74512-166">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="74512-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_configure.png) 

7. <span data-ttu-id="74512-168">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na vaše [Novatus tým podpory](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="74512-168">To get SSO configured for your application, contact your [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> <span data-ttu-id="74512-169">Připojit **stáhnout certifikát** souborů k e-mailu a sdílené složky **adres URL metadat** (**Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby**) s týmem Novatus na jejich straně nastavit jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="74512-169">Attach the **downloaded certificate** file to your mail and share the **metadata urls** (**Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL**) with Novatus team to set up SSO on their side.</span></span>

> [!TIP]
> <span data-ttu-id="74512-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="74512-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="74512-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="74512-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="74512-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="74512-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="74512-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="74512-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="74512-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74512-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="74512-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74512-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="74512-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="74512-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="74512-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="74512-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="74512-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74512-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="74512-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="74512-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="74512-185">a.</span><span class="sxs-lookup"><span data-stu-id="74512-185">a.</span></span> <span data-ttu-id="74512-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="74512-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="74512-187">b.</span><span class="sxs-lookup"><span data-stu-id="74512-187">b.</span></span> <span data-ttu-id="74512-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="74512-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="74512-189">c.</span><span class="sxs-lookup"><span data-stu-id="74512-189">c.</span></span> <span data-ttu-id="74512-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="74512-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="74512-191">d.</span><span class="sxs-lookup"><span data-stu-id="74512-191">d.</span></span> <span data-ttu-id="74512-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="74512-192">Click **Create**.</span></span>
 
### <a name="creating-a-novatus-test-user"></a><span data-ttu-id="74512-193">Vytvoření zkušebního uživatele Novatus</span><span class="sxs-lookup"><span data-stu-id="74512-193">Creating a Novatus test user</span></span>

<span data-ttu-id="74512-194">Cílem této části je vytvoření uživatele v Novatus nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74512-194">The objective of this section is to create a user called Britta Simon in Novatus.</span></span> <span data-ttu-id="74512-195">Novatus podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="74512-195">Novatus supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="74512-196">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="74512-196">There is no action item for you in this section.</span></span> <span data-ttu-id="74512-197">Vytvoří se nový uživatel během pokusu o přístup k Novatus, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="74512-197">A new user will be created during an attempt to access Novatus if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="74512-198">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktovat [tým podpory Novatus](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="74512-198">If you need to create an user manually, you need to contact the [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="74512-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="74512-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="74512-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Novatus.</span><span class="sxs-lookup"><span data-stu-id="74512-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Novatus.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="74512-202">**Pokud chcete přiřadit Britta Simon Novatus, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74512-202">**To assign Britta Simon to Novatus, perform the following steps:**</span></span>

1. <span data-ttu-id="74512-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="74512-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="74512-205">V seznamu aplikací vyberte **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="74512-205">In the applications list, select **Novatus**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_app.png) 

3. <span data-ttu-id="74512-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="74512-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="74512-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74512-209">Click **Add** button.</span></span> <span data-ttu-id="74512-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74512-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="74512-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="74512-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="74512-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74512-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="74512-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74512-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="74512-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="74512-215">Testing single sign-on</span></span>

<span data-ttu-id="74512-216">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="74512-216">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="74512-217">Když kliknete na dlaždici Novatus na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Novatus.</span><span class="sxs-lookup"><span data-stu-id="74512-217">When you click the Novatus tile in the Access Panel, you should get automatically signed-on to your Novatus application.</span></span> <span data-ttu-id="74512-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="74512-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74512-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="74512-219">Additional resources</span></span>

* [<span data-ttu-id="74512-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="74512-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="74512-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="74512-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_203.png

