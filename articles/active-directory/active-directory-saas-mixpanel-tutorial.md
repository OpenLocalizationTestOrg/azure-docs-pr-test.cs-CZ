---
title: 'Kurz: Azure Active Directory integrace s Mixpanel | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Mixpanel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3dd11b3477de1329c1c8e45a6dbf212b1635fd95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a><span data-ttu-id="2880f-103">Kurz: Azure Active Directory integrace s Mixpanel</span><span class="sxs-lookup"><span data-stu-id="2880f-103">Tutorial: Azure Active Directory integration with Mixpanel</span></span>

<span data-ttu-id="2880f-104">V tomto kurzu zjistěte, jak integrovat Mixpanel s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2880f-104">In this tutorial, you learn how to integrate Mixpanel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2880f-105">Integrace Mixpanel s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2880f-105">Integrating Mixpanel with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2880f-106">Můžete řídit ve službě Azure AD, který má přístup k Mixpanel</span><span class="sxs-lookup"><span data-stu-id="2880f-106">You can control in Azure AD who has access to Mixpanel</span></span>
- <span data-ttu-id="2880f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Mixpanel (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2880f-107">You can enable your users to automatically get signed-on to Mixpanel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2880f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2880f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2880f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2880f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2880f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2880f-110">Prerequisites</span></span>

<span data-ttu-id="2880f-111">Konfigurace integrace Azure AD s Mixpanel, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2880f-111">To configure Azure AD integration with Mixpanel, you need the following items:</span></span>

- <span data-ttu-id="2880f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2880f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2880f-113">Mixpanel jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2880f-113">A Mixpanel single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2880f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2880f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2880f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2880f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2880f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2880f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2880f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2880f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2880f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2880f-118">Scenario description</span></span>
<span data-ttu-id="2880f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2880f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2880f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2880f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2880f-121">Přidání Mixpanel z Galerie</span><span class="sxs-lookup"><span data-stu-id="2880f-121">Adding Mixpanel from the gallery</span></span>
2. <span data-ttu-id="2880f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2880f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mixpanel-from-the-gallery"></a><span data-ttu-id="2880f-123">Přidání Mixpanel z Galerie</span><span class="sxs-lookup"><span data-stu-id="2880f-123">Adding Mixpanel from the gallery</span></span>
<span data-ttu-id="2880f-124">Při konfiguraci integrace Mixpanel do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Mixpanel z galerie.</span><span class="sxs-lookup"><span data-stu-id="2880f-124">To configure the integration of Mixpanel into Azure AD, you need to add Mixpanel from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2880f-125">**Pokud chcete přidat Mixpanel z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2880f-125">**To add Mixpanel from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2880f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2880f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2880f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2880f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2880f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2880f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2880f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2880f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2880f-133">Do vyhledávacího pole zadejte **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="2880f-133">In the search box, type **Mixpanel**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_search.png)

5. <span data-ttu-id="2880f-135">Na panelu výsledků vyberte **Mixpanel**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2880f-135">In the results panel, select **Mixpanel**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2880f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2880f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2880f-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mixpanel podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2880f-138">In this section, you configure and test Azure AD single sign-on with Mixpanel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2880f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Mixpanel je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2880f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mixpanel is to a user in Azure AD.</span></span> <span data-ttu-id="2880f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Mixpanel musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2880f-140">In other words, a link relationship between an Azure AD user and the related user in Mixpanel needs to be established.</span></span>

<span data-ttu-id="2880f-141">V Mixpanel, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="2880f-141">In Mixpanel, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2880f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mixpanel, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2880f-142">To configure and test Azure AD single sign-on with Mixpanel, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2880f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2880f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2880f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2880f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2880f-145">**[Vytvoření zkušebního uživatele Mixpanel](#creating-a-mixpanel-test-user)**  – Pokud chcete mít protějšek Britta Simon v Mixpanel propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2880f-145">**[Creating a Mixpanel test user](#creating-a-mixpanel-test-user)** - to have a counterpart of Britta Simon in Mixpanel that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2880f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2880f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2880f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2880f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2880f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2880f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2880f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="2880f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mixpanel application.</span></span>

<span data-ttu-id="2880f-150">**Ke konfiguraci Azure AD jednotné přihlašování s Mixpanel, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2880f-150">**To configure Azure AD single sign-on with Mixpanel, perform the following steps:**</span></span>

1. <span data-ttu-id="2880f-151">Na portálu Azure na **Mixpanel** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2880f-151">In the Azure portal, on the **Mixpanel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2880f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2880f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

3. <span data-ttu-id="2880f-155">Na **Mixpanel domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2880f-155">On the **Mixpanel Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_url.png)

     <span data-ttu-id="2880f-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://mixpanel.com/login/`</span><span class="sxs-lookup"><span data-stu-id="2880f-157">In the **Sign-on URL** textbox, type a URL as: `https://mixpanel.com/login/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2880f-158">Zaregistrujte se prosím na [https://mixpanel.com/register/](https://mixpanel.com/register/) nastavit přihlašovací údaje a obraťte se [tým podpory Mixpanel](mailto:support@mixpanel.com) umožňující jednotného přihlašování k nastavení pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="2880f-158">Please register at [https://mixpanel.com/register/](https://mixpanel.com/register/) to set up your login credentials and  contact the [Mixpanel support team](mailto:support@mixpanel.com) to enable SSO settings for your tenant.</span></span> <span data-ttu-id="2880f-159">Hodnota přihlašovací adresa URL můžete také získat v případě potřeby váš tým podpory Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="2880f-159">You can also get your Sign On URL value if necessary from your Mixpanel support team.</span></span> 
 
4. <span data-ttu-id="2880f-160">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="2880f-160">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

5. <span data-ttu-id="2880f-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2880f-162">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2880f-164">Na **Mixpanel konfigurace** klikněte na tlačítko **konfigurace Mixpanel** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2880f-164">On the **Mixpanel Configuration** section, click **Configure Mixpanel** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2880f-165">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="2880f-165">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_configure.png) 

7. <span data-ttu-id="2880f-167">V okně jiný prohlížeč přihlášení do aplikace Mixpanel jako správce.</span><span class="sxs-lookup"><span data-stu-id="2880f-167">In a different browser window, sign-on to your Mixpanel application as an administrator.</span></span>

8. <span data-ttu-id="2880f-168">V dolní části stránky klikněte na tlačítko málo **zařízení** ikona levém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="2880f-168">On bottom of the page, click the little **gear** icon in the left corner.</span></span> 
   
    ![Mixpanel jednotné přihlášení](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

9. <span data-ttu-id="2880f-170">Klikněte **zabezpečení přístupu k** a pak klikněte **změnit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2880f-170">Click the **Access security** tab, and then click **Change settings**.</span></span>
   
    ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 

10. <span data-ttu-id="2880f-172">Na **změnit svůj certifikát** dialogové okno stránky, klikněte na tlačítko **zvolte soubor** stažený certifikát, a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2880f-172">On the **Change your certificate** dialog page, click **Choose file** to upload your downloaded certificate, and then click **NEXT**.</span></span>
   
    ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

11.  <span data-ttu-id="2880f-174">Ověřování adresy URL do textového pole na **změnit adresu URL ověřování** dialogové okno stránky, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2880f-174">In the authentication URL textbox on the **Change your authentication  URL** dialog page, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal, and then click **NEXT**.</span></span>
   
   ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 

12. <span data-ttu-id="2880f-176">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="2880f-176">Click **Done**.</span></span>

> [!TIP]
> <span data-ttu-id="2880f-177">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2880f-177">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2880f-178">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="2880f-178">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2880f-179">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2880f-179">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2880f-180">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2880f-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="2880f-181">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2880f-181">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2880f-183">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2880f-183">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2880f-184">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2880f-184">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2880f-186">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2880f-186">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2880f-188">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2880f-188">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2880f-190">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2880f-190">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2880f-192">a.</span><span class="sxs-lookup"><span data-stu-id="2880f-192">a.</span></span> <span data-ttu-id="2880f-193">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2880f-193">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2880f-194">b.</span><span class="sxs-lookup"><span data-stu-id="2880f-194">b.</span></span> <span data-ttu-id="2880f-195">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2880f-195">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2880f-196">c.</span><span class="sxs-lookup"><span data-stu-id="2880f-196">c.</span></span> <span data-ttu-id="2880f-197">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2880f-197">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2880f-198">d.</span><span class="sxs-lookup"><span data-stu-id="2880f-198">d.</span></span> <span data-ttu-id="2880f-199">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2880f-199">Click **Create**.</span></span>
 
### <a name="creating-a-mixpanel-test-user"></a><span data-ttu-id="2880f-200">Vytvoření zkušebního uživatele Mixpanel</span><span class="sxs-lookup"><span data-stu-id="2880f-200">Creating a Mixpanel test user</span></span>

<span data-ttu-id="2880f-201">Cílem této části je vytvoření uživatele v Mixpanel nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2880f-201">The objective of this section is to create a user called Britta Simon in Mixpanel.</span></span> 

1. <span data-ttu-id="2880f-202">Přihlásit se k serveru vaší společnosti Mixpanel jako správce.</span><span class="sxs-lookup"><span data-stu-id="2880f-202">Sign on to your Mixpanel company site as an administrator.</span></span>

2. <span data-ttu-id="2880f-203">V dolní části stránky, klikněte na tlačítko málo ozubené kolečko na levém rohu otevřete **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="2880f-203">On the bottom of the page, click the little gear button on the left corner to open the **Settings** window.</span></span>

3. <span data-ttu-id="2880f-204">Klikněte **Team** kartě.</span><span class="sxs-lookup"><span data-stu-id="2880f-204">Click the **Team** tab.</span></span>

4. <span data-ttu-id="2880f-205">V **člen týmu** textovému poli, zadejte na Britta e-mailovou adresu ve službě Azure.</span><span class="sxs-lookup"><span data-stu-id="2880f-205">In the **team member** textbox, type Britta's email address in the Azure.</span></span>
   
    ![Nastavení Mixpanel](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. <span data-ttu-id="2880f-207">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="2880f-207">Click **Invite**.</span></span> 

> [!Note]
> <span data-ttu-id="2880f-208">Uživatel dostane e-mail na vytvoření profilu.</span><span class="sxs-lookup"><span data-stu-id="2880f-208">The user will get an email to set up the profile.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2880f-209">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2880f-209">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2880f-210">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="2880f-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mixpanel.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2880f-212">**Pokud chcete přiřadit Britta Simon Mixpanel, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2880f-212">**To assign Britta Simon to Mixpanel, perform the following steps:**</span></span>

1. <span data-ttu-id="2880f-213">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2880f-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2880f-215">V seznamu aplikací vyberte **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="2880f-215">In the applications list, select **Mixpanel**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_app.png) 

3. <span data-ttu-id="2880f-217">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2880f-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2880f-219">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2880f-219">Click **Add** button.</span></span> <span data-ttu-id="2880f-220">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2880f-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2880f-222">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2880f-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2880f-223">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2880f-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2880f-224">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2880f-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2880f-225">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2880f-225">Testing single sign-on</span></span>

<span data-ttu-id="2880f-226">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2880f-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2880f-227">Když kliknete na dlaždici Mixpanel na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="2880f-227">When you click the Mixpanel tile in the Access Panel, you should get automatically signed-on to your Mixpanel application.</span></span>
<span data-ttu-id="2880f-228">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2880f-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2880f-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2880f-229">Additional resources</span></span>

* [<span data-ttu-id="2880f-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2880f-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2880f-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2880f-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png

