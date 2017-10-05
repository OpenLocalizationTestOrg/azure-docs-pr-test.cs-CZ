---
title: 'Kurz: Azure Active Directory integrace s RunMyProcess | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a RunMyProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f8a08ef4f90d5cb98e7648ae6001055a3f4696e8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a><span data-ttu-id="44846-103">Kurz: Azure Active Directory integrace s RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="44846-103">Tutorial: Azure Active Directory integration with RunMyProcess</span></span>

<span data-ttu-id="44846-104">V tomto kurzu zjistěte, jak integrovat RunMyProcess s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="44846-104">In this tutorial, you learn how to integrate RunMyProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="44846-105">Integrace RunMyProcess s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="44846-105">Integrating RunMyProcess with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="44846-106">Můžete řídit ve službě Azure AD, který má přístup k RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="44846-106">You can control in Azure AD who has access to RunMyProcess</span></span>
- <span data-ttu-id="44846-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k RunMyProcess (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="44846-107">You can enable your users to automatically get signed-on to RunMyProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="44846-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="44846-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="44846-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="44846-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44846-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="44846-110">Prerequisites</span></span>

<span data-ttu-id="44846-111">Konfigurace integrace Azure AD s RunMyProcess, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="44846-111">To configure Azure AD integration with RunMyProcess, you need the following items:</span></span>

- <span data-ttu-id="44846-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="44846-112">An Azure AD subscription</span></span>
- <span data-ttu-id="44846-113">RunMyProcess jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="44846-113">A RunMyProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="44846-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="44846-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="44846-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="44846-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="44846-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="44846-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="44846-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební:[nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="44846-117">If you don't have an Azure AD trial environment, you can get a one-month trial here:[Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="44846-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="44846-118">Scenario description</span></span>
<span data-ttu-id="44846-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="44846-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="44846-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="44846-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="44846-121">Přidání RunMyProcess z Galerie</span><span class="sxs-lookup"><span data-stu-id="44846-121">Adding RunMyProcess from the gallery</span></span>
2. <span data-ttu-id="44846-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="44846-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-runmyprocess-from-the-gallery"></a><span data-ttu-id="44846-123">Přidání RunMyProcess z Galerie</span><span class="sxs-lookup"><span data-stu-id="44846-123">Adding RunMyProcess from the gallery</span></span>
<span data-ttu-id="44846-124">Při konfiguraci integrace RunMyProcess do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS RunMyProcess z galerie.</span><span class="sxs-lookup"><span data-stu-id="44846-124">To configure the integration of RunMyProcess into Azure AD, you need to add RunMyProcess from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="44846-125">**Pokud chcete přidat RunMyProcess z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44846-125">**To add RunMyProcess from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="44846-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="44846-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="44846-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="44846-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="44846-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="44846-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="44846-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44846-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="44846-133">Do vyhledávacího pole zadejte **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="44846-133">In the search box, type **RunMyProcess**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_search.png)

5. <span data-ttu-id="44846-135">Na panelu výsledků vyberte **RunMyProcess**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="44846-135">In the results panel, select **RunMyProcess**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="44846-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="44846-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="44846-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s RunMyProcess podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="44846-138">In this section, you configure and test Azure AD single sign-on with RunMyProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="44846-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v RunMyProcess je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="44846-139">For single sign-on to work, Azure AD needs to know what the counterpart user in RunMyProcess is to a user in Azure AD.</span></span> <span data-ttu-id="44846-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v RunMyProcess musí navázat.</span><span class="sxs-lookup"><span data-stu-id="44846-140">In other words, a link relationship between an Azure AD user and the related user in RunMyProcess needs to be established.</span></span>

<span data-ttu-id="44846-141">V RunMyProcess, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="44846-141">In RunMyProcess, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="44846-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s RunMyProcess, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="44846-142">To configure and test Azure AD single sign-on with RunMyProcess, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="44846-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="44846-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="44846-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44846-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="44846-145">**[Vytvoření zkušebního uživatele RunMyProcess](#creating-a-runmyprocess-test-user)**  – Pokud chcete mít protějšek Britta Simon v RunMyProcess propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="44846-145">**[Creating a RunMyProcess test user](#creating-a-runmyprocess-test-user)** - to have a counterpart of Britta Simon in RunMyProcess that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="44846-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="44846-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="44846-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="44846-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="44846-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="44846-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="44846-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="44846-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RunMyProcess application.</span></span>

<span data-ttu-id="44846-150">**Ke konfiguraci Azure AD jednotné přihlašování s RunMyProcess, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44846-150">**To configure Azure AD single sign-on with RunMyProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="44846-151">Na portálu Azure na **RunMyProcess** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="44846-151">In the Azure portal, on the **RunMyProcess** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="44846-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="44846-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

3. <span data-ttu-id="44846-155">Na **RunMyProcess domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44846-155">On the **RunMyProcess Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    <span data-ttu-id="44846-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://live.runmyprocess.com/live/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="44846-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://live.runmyprocess.com/live/<tenant id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="44846-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="44846-158">The value is not real.</span></span> <span data-ttu-id="44846-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="44846-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="44846-160">Obraťte se na [tým podpory RunMyProcess klienta](mailto:support@runmyprocess.com) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="44846-160">Contact [RunMyProcess Client support team](mailto:support@runmyprocess.com) to get the value.</span></span> 

4. <span data-ttu-id="44846-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="44846-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

5. <span data-ttu-id="44846-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="44846-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="44846-165">Na **RunMyProcess konfigurace** klikněte na tlačítko **konfigurace RunMyProcess** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="44846-165">On the **RunMyProcess Configuration** section, click **Configure RunMyProcess** to open **Configure sign-on** window.</span></span> <span data-ttu-id="44846-166">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="44846-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

7. <span data-ttu-id="44846-168">V okně prohlížeče jiných webových přihlašování ke klientovi RunMyProcess jako správce.</span><span class="sxs-lookup"><span data-stu-id="44846-168">In a different web browser window, sign-on to your RunMyProcess tenant as an administrator.</span></span>

8. <span data-ttu-id="44846-169">V levém navigačním panelu klikněte na **účet** a vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="44846-169">In left navigation panel, click **Account** and select **Configuration**.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

9. <span data-ttu-id="44846-171">Přejděte na **metodu ověřování** části a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44846-171">Go to **Authentication method** section and perform below steps:</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    <span data-ttu-id="44846-173">a.</span><span class="sxs-lookup"><span data-stu-id="44846-173">a.</span></span> <span data-ttu-id="44846-174">Jako **metoda**, vyberte **přihlášení SSO se Samlv2**.</span><span class="sxs-lookup"><span data-stu-id="44846-174">As **Method**, select **SSO with Samlv2**.</span></span> 

    <span data-ttu-id="44846-175">b.</span><span class="sxs-lookup"><span data-stu-id="44846-175">b.</span></span> <span data-ttu-id="44846-176">V **jednotného přihlašování k přesměrování** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44846-176">In the **SSO redirect** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="44846-177">c.</span><span class="sxs-lookup"><span data-stu-id="44846-177">c.</span></span> <span data-ttu-id="44846-178">V **přesměrování odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="44846-178">In the **Logout redirect** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="44846-179">d.</span><span class="sxs-lookup"><span data-stu-id="44846-179">d.</span></span> <span data-ttu-id="44846-180">V **formát Id názvu** textovému poli, zadejte hodnotu **formát názvu identifikátor** jako **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="44846-180">In the **Name Id Format** textbox, type the value of **Name Identifier Format** as **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="44846-181">e.</span><span class="sxs-lookup"><span data-stu-id="44846-181">e.</span></span> <span data-ttu-id="44846-182">Zkopírujte obsah soubor stažený certifikát a vložte ji do **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="44846-182">Copy the content of the downloaded certificate file and then paste it into the **Certificate** textbox.</span></span> 
 
    <span data-ttu-id="44846-183">f.</span><span class="sxs-lookup"><span data-stu-id="44846-183">f.</span></span> <span data-ttu-id="44846-184">Klikněte na tlačítko **Uložit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="44846-184">Click **Save** icon.</span></span>

> [!TIP]
> <span data-ttu-id="44846-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="44846-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="44846-186">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="44846-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="44846-187">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="44846-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="44846-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="44846-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="44846-189">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44846-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="44846-191">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44846-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="44846-192">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="44846-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="44846-194">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="44846-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="44846-196">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44846-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="44846-198">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44846-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="44846-200">a.</span><span class="sxs-lookup"><span data-stu-id="44846-200">a.</span></span> <span data-ttu-id="44846-201">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="44846-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="44846-202">b.</span><span class="sxs-lookup"><span data-stu-id="44846-202">b.</span></span> <span data-ttu-id="44846-203">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="44846-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="44846-204">c.</span><span class="sxs-lookup"><span data-stu-id="44846-204">c.</span></span> <span data-ttu-id="44846-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="44846-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="44846-206">d.</span><span class="sxs-lookup"><span data-stu-id="44846-206">d.</span></span> <span data-ttu-id="44846-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="44846-207">Click **Create**.</span></span>
 
### <a name="creating-a-runmyprocess-test-user"></a><span data-ttu-id="44846-208">Vytvoření zkušebního uživatele RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="44846-208">Creating a RunMyProcess test user</span></span>

<span data-ttu-id="44846-209">Pokud chcete povolit uživatelům Azure AD přihlášení k RunMyProcess, musí být zřízená do RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="44846-209">In order to enable Azure AD users to log in to RunMyProcess, they must be provisioned into RunMyProcess.</span></span> <span data-ttu-id="44846-210">V případě RunMyProcess zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="44846-210">In the case of RunMyProcess, provisioning is a manual task.</span></span>

<span data-ttu-id="44846-211">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44846-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="44846-212">Přihlaste se k serveru vaší společnosti RunMyProcess jako správce.</span><span class="sxs-lookup"><span data-stu-id="44846-212">Log in to your RunMyProcess company site as an administrator.</span></span>

2. <span data-ttu-id="44846-213">Klikněte na tlačítko **účet** a vyberte **uživatelé** v levém navigačním panelu klikněte **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="44846-213">Click **Account** and select **Users** in left navigation panel, then click **New User**.</span></span>
   
    <span data-ttu-id="44846-214">![Nový uživatel](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="44846-214">![New User](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "New User")</span></span>

3. <span data-ttu-id="44846-215">V **uživatelská nastavení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44846-215">In the **User Settings** section, perform the following steps:</span></span>
   
    <span data-ttu-id="44846-216">![Profil](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "profilu")</span><span class="sxs-lookup"><span data-stu-id="44846-216">![Profile](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profile")</span></span> 
  
    <span data-ttu-id="44846-217">a.</span><span class="sxs-lookup"><span data-stu-id="44846-217">a.</span></span> <span data-ttu-id="44846-218">Typ **název** a **e-mailu** platný Azure AD účtu chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="44846-218">Type the **Name** and **E-mail** of a valid Azure AD account you want to provision into the related textboxes.</span></span> 

    <span data-ttu-id="44846-219">b.</span><span class="sxs-lookup"><span data-stu-id="44846-219">b.</span></span> <span data-ttu-id="44846-220">Vyberte **IDE jazyk**, **jazyk**, a **profil**.</span><span class="sxs-lookup"><span data-stu-id="44846-220">Select an **IDE language**, **Language**, and **Profile**.</span></span> 

    <span data-ttu-id="44846-221">c.</span><span class="sxs-lookup"><span data-stu-id="44846-221">c.</span></span> <span data-ttu-id="44846-222">Vyberte **účet vytvoření e-mailu Poslat mi**.</span><span class="sxs-lookup"><span data-stu-id="44846-222">Select **Send account creation e-mail to me**.</span></span> 

    <span data-ttu-id="44846-223">d.</span><span class="sxs-lookup"><span data-stu-id="44846-223">d.</span></span> <span data-ttu-id="44846-224">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="44846-224">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="44846-225">Můžete použít všechny ostatní RunMyProcess uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované RunMyProcess zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="44846-225">You can use any other RunMyProcess user account creation tools or APIs provided by RunMyProcess to provision Azure Active Directory user accounts.</span></span> 
    > 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="44846-226">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="44846-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="44846-227">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="44846-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RunMyProcess.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="44846-229">**Pokud chcete přiřadit Britta Simon RunMyProcess, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="44846-229">**To assign Britta Simon to RunMyProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="44846-230">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="44846-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="44846-232">V seznamu aplikací vyberte **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="44846-232">In the applications list, select **RunMyProcess**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

3. <span data-ttu-id="44846-234">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="44846-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="44846-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="44846-236">Click **Add** button.</span></span> <span data-ttu-id="44846-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44846-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="44846-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="44846-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="44846-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44846-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="44846-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="44846-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="44846-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="44846-242">Testing single sign-on</span></span>

<span data-ttu-id="44846-243">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="44846-243">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="44846-244">Když kliknete na dlaždici RunMyProcess na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="44846-244">When you click the RunMyProcess tile in the Access Panel, you should get automatically signed-on to your RunMyProcess application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="44846-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="44846-245">Additional resources</span></span>

* [<span data-ttu-id="44846-246">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44846-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="44846-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="44846-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png

