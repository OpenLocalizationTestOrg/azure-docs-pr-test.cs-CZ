---
title: 'Kurz: Azure Active Directory integrace s Printix | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Printix."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 97dbb3fa0531f2f679badb6bb9752f2e42fc9cb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a><span data-ttu-id="81eac-103">Kurz: Azure Active Directory integrace s Printix</span><span class="sxs-lookup"><span data-stu-id="81eac-103">Tutorial: Azure Active Directory integration with Printix</span></span>

<span data-ttu-id="81eac-104">V tomto kurzu zjistěte, jak integrovat Printix s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="81eac-104">In this tutorial, you learn how to integrate Printix with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="81eac-105">Integrace Printix s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="81eac-105">Integrating Printix with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="81eac-106">Můžete řídit ve službě Azure AD, který má přístup k Printix</span><span class="sxs-lookup"><span data-stu-id="81eac-106">You can control in Azure AD who has access to Printix</span></span>
- <span data-ttu-id="81eac-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Printix (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="81eac-107">You can enable your users to automatically get signed-on to Printix (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="81eac-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="81eac-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="81eac-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="81eac-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81eac-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="81eac-110">Prerequisites</span></span>

<span data-ttu-id="81eac-111">Konfigurace integrace Azure AD s Printix, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="81eac-111">To configure Azure AD integration with Printix, you need the following items:</span></span>

- <span data-ttu-id="81eac-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="81eac-112">An Azure AD subscription</span></span>
- <span data-ttu-id="81eac-113">Printix jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="81eac-113">A Printix single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="81eac-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="81eac-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="81eac-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="81eac-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="81eac-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="81eac-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="81eac-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="81eac-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="81eac-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="81eac-118">Scenario description</span></span>
<span data-ttu-id="81eac-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="81eac-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="81eac-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="81eac-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="81eac-121">Přidání Printix z Galerie</span><span class="sxs-lookup"><span data-stu-id="81eac-121">Adding Printix from the gallery</span></span>
2. <span data-ttu-id="81eac-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="81eac-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-printix-from-the-gallery"></a><span data-ttu-id="81eac-123">Přidání Printix z Galerie</span><span class="sxs-lookup"><span data-stu-id="81eac-123">Adding Printix from the gallery</span></span>
<span data-ttu-id="81eac-124">Při konfiguraci integrace Printix do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Printix z galerie.</span><span class="sxs-lookup"><span data-stu-id="81eac-124">To configure the integration of Printix into Azure AD, you need to add Printix from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="81eac-125">**Pokud chcete přidat Printix z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="81eac-125">**To add Printix from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="81eac-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="81eac-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="81eac-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="81eac-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="81eac-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="81eac-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="81eac-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="81eac-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="81eac-133">Do vyhledávacího pole zadejte **Printix**.</span><span class="sxs-lookup"><span data-stu-id="81eac-133">In the search box, type **Printix**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. <span data-ttu-id="81eac-135">Na panelu výsledků vyberte **Printix**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="81eac-135">In the results panel, select **Printix**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="81eac-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="81eac-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="81eac-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Printix podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="81eac-138">In this section, you configure and test Azure AD single sign-on with Printix based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="81eac-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Printix je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="81eac-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Printix is to a user in Azure AD.</span></span> <span data-ttu-id="81eac-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Printix musí navázat.</span><span class="sxs-lookup"><span data-stu-id="81eac-140">In other words, a link relationship between an Azure AD user and the related user in Printix needs to be established.</span></span>

<span data-ttu-id="81eac-141">V Printix, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="81eac-141">In Printix, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="81eac-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Printix, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="81eac-142">To configure and test Azure AD single sign-on with Printix, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="81eac-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="81eac-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="81eac-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="81eac-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="81eac-145">**[Vytvoření zkušebního uživatele Printix](#creating-a-printix-test-user)**  – Pokud chcete mít protějšek Britta Simon v Printix propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="81eac-145">**[Creating a Printix test user](#creating-a-printix-test-user)** - to have a counterpart of Britta Simon in Printix that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="81eac-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="81eac-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="81eac-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="81eac-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="81eac-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="81eac-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="81eac-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Printix.</span><span class="sxs-lookup"><span data-stu-id="81eac-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Printix application.</span></span>

<span data-ttu-id="81eac-150">**Ke konfiguraci Azure AD jednotné přihlašování s Printix, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="81eac-150">**To configure Azure AD single sign-on with Printix, perform the following steps:**</span></span>

1. <span data-ttu-id="81eac-151">Na portálu Azure na **Printix** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="81eac-151">In the Azure portal, on the **Printix** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="81eac-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="81eac-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. <span data-ttu-id="81eac-155">Na **Printix domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="81eac-155">On the **Printix Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    <span data-ttu-id="81eac-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.printix.net`</span><span class="sxs-lookup"><span data-stu-id="81eac-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.printix.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="81eac-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="81eac-158">The value is not real.</span></span> <span data-ttu-id="81eac-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="81eac-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="81eac-160">Obraťte se na [tým podpory Printix klienta](mailto:support@printix.net) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="81eac-160">Contact [Printix Client support team](mailto:support@printix.net) to get the value.</span></span> 
 
4. <span data-ttu-id="81eac-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="81eac-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. <span data-ttu-id="81eac-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="81eac-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="81eac-165">Přihlášení ke klientovi Printix jako správce.</span><span class="sxs-lookup"><span data-stu-id="81eac-165">Sign-on to your Printix tenant as an administrator.</span></span>

7. <span data-ttu-id="81eac-166">V nabídce v horní části, klikněte na ikonu v pravém horním rohu a vyberte "**ověřování**".</span><span class="sxs-lookup"><span data-stu-id="81eac-166">In the menu on the top, click the icon at the upper right corner and select "**Authentication**".</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. <span data-ttu-id="81eac-168">Na **instalační program** vyberte **Azure nebo Office 365 povolit ověřování**</span><span class="sxs-lookup"><span data-stu-id="81eac-168">On the **Setup** tab, select **Enable Azure/Office 365 authentication**</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. <span data-ttu-id="81eac-170">Na **Azure** kartě, zadejte adresu URL federačních metadat do textového pole z "**dokument federačních metadat**".</span><span class="sxs-lookup"><span data-stu-id="81eac-170">On the **Azure** tab, input federation metadata URL to the textbox of "**Federation metadata document**".</span></span> 

    <span data-ttu-id="81eac-171">Připojte soubor metadat xml, který jste si stáhli z Azure AD [tým podpory Printix](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="81eac-171">Attach the metadata xml file which you downloaded from Azure AD to [Printix support team](mailto:support@printix.net).</span></span> <span data-ttu-id="81eac-172">Pak nahrajte soubor xml a zadat adresu URL federačních metadat.</span><span class="sxs-lookup"><span data-stu-id="81eac-172">Then they upload the xml file and provide a federation metadata URL.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. <span data-ttu-id="81eac-174">Klikněte na tlačítko "**testování**"tlačítko a klikněte na tlačítko"**OK**" tlačítko, pokud byl test úspěšný.</span><span class="sxs-lookup"><span data-stu-id="81eac-174">Click the "**Test**" button and click "**OK**" button if the test was successful.</span></span>
   
     <span data-ttu-id="81eac-175">Po kliknutí na tlačítko se zobrazí stránka služby Azure active directory **testování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="81eac-175">Azure active directory page will show after clicking the **test** button.</span></span> <span data-ttu-id="81eac-176">"Test bylo úspěšné" zde znamená po zadání přihlašovacích údajů účtu Azure test, který bude pop se zobrazí zpráva "nastavení testovaný OK". Klikněte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="81eac-176">"The test was successful" here means after entering the credentials of your Azure test account it will pop up a message "Settings tested OK".Then click the **OK** button.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. <span data-ttu-id="81eac-178">Klikněte **Uložit** na tlačítko "**ověřování**" stránky.</span><span class="sxs-lookup"><span data-stu-id="81eac-178">Click the **Save** button on "**Authentication**" page.</span></span>


> [!TIP]
> <span data-ttu-id="81eac-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="81eac-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="81eac-180">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="81eac-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="81eac-181">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="81eac-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="81eac-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="81eac-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="81eac-183">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="81eac-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="81eac-185">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="81eac-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="81eac-186">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="81eac-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="81eac-188">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="81eac-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="81eac-190">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="81eac-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="81eac-192">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="81eac-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="81eac-194">a.</span><span class="sxs-lookup"><span data-stu-id="81eac-194">a.</span></span> <span data-ttu-id="81eac-195">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="81eac-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="81eac-196">b.</span><span class="sxs-lookup"><span data-stu-id="81eac-196">b.</span></span> <span data-ttu-id="81eac-197">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="81eac-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="81eac-198">c.</span><span class="sxs-lookup"><span data-stu-id="81eac-198">c.</span></span> <span data-ttu-id="81eac-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="81eac-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="81eac-200">d.</span><span class="sxs-lookup"><span data-stu-id="81eac-200">d.</span></span> <span data-ttu-id="81eac-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="81eac-201">Click **Create**.</span></span>
 
### <a name="creating-a-printix-test-user"></a><span data-ttu-id="81eac-202">Vytvoření zkušebního uživatele Printix</span><span class="sxs-lookup"><span data-stu-id="81eac-202">Creating a Printix test user</span></span>

<span data-ttu-id="81eac-203">Cílem této části je vytvoření uživatele v Printix nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="81eac-203">The objective of this section is to create a user called Britta Simon in Printix.</span></span> <span data-ttu-id="81eac-204">Printix podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="81eac-204">Printix supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="81eac-205">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="81eac-205">There is no action item for you in this section.</span></span> <span data-ttu-id="81eac-206">Nový uživatel se vytvoří během pokusu o přístup k Printix, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="81eac-206">A new user is created during an attempt to access Printix if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="81eac-207">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Printix](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="81eac-207">If you need to create a user manually, you need to contact the [Printix support team](mailto:support@printix.net).</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="81eac-208">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="81eac-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="81eac-209">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Printix.</span><span class="sxs-lookup"><span data-stu-id="81eac-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Printix.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="81eac-211">**Pokud chcete přiřadit Britta Simon Printix, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="81eac-211">**To assign Britta Simon to Printix, perform the following steps:**</span></span>

1. <span data-ttu-id="81eac-212">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="81eac-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="81eac-214">V seznamu aplikací vyberte **Printix**.</span><span class="sxs-lookup"><span data-stu-id="81eac-214">In the applications list, select **Printix**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. <span data-ttu-id="81eac-216">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="81eac-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="81eac-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="81eac-218">Click **Add** button.</span></span> <span data-ttu-id="81eac-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="81eac-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="81eac-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="81eac-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="81eac-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="81eac-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="81eac-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="81eac-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="81eac-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="81eac-224">Testing single sign-on</span></span>

<span data-ttu-id="81eac-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="81eac-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="81eac-226">Když kliknete na dlaždici Printix na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Printix.</span><span class="sxs-lookup"><span data-stu-id="81eac-226">When you click the Printix tile in the Access Panel, you should get automatically signed-on to your Printix application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="81eac-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="81eac-227">Additional resources</span></span>

* [<span data-ttu-id="81eac-228">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="81eac-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="81eac-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="81eac-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

