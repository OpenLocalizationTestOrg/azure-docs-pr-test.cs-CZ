---
title: 'Kurz: Azure Active Directory integrace s FilesAnywhere | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a FilesAnywhere."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 4153056bd21006061c6ad8ff9cf3c17de9248628
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a><span data-ttu-id="0c430-103">Kurz: Azure Active Directory integrace s FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="0c430-103">Tutorial: Azure Active Directory integration with FilesAnywhere</span></span>

<span data-ttu-id="0c430-104">V tomto kurzu zjistěte, jak integrovat FilesAnywhere s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0c430-104">In this tutorial, you learn how to integrate FilesAnywhere with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0c430-105">Integrace FilesAnywhere s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0c430-105">Integrating FilesAnywhere with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0c430-106">Můžete řídit ve službě Azure AD, který má přístup k FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="0c430-106">You can control in Azure AD who has access to FilesAnywhere</span></span>
- <span data-ttu-id="0c430-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k FilesAnywhere (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0c430-107">You can enable your users to automatically get signed-on to FilesAnywhere (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0c430-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="0c430-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="0c430-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0c430-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c430-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0c430-110">Prerequisites</span></span>

<span data-ttu-id="0c430-111">Konfigurace integrace Azure AD s FilesAnywhere, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0c430-111">To configure Azure AD integration with FilesAnywhere, you need the following items:</span></span>

- <span data-ttu-id="0c430-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0c430-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0c430-113">FilesAnywhere jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0c430-113">A FilesAnywhere single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="0c430-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0c430-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="0c430-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0c430-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0c430-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="0c430-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="0c430-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0c430-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="0c430-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0c430-118">Scenario description</span></span>
<span data-ttu-id="0c430-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0c430-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0c430-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0c430-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0c430-121">Přidání FilesAnywhere z Galerie</span><span class="sxs-lookup"><span data-stu-id="0c430-121">Adding FilesAnywhere from the gallery</span></span>
2. <span data-ttu-id="0c430-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0c430-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-filesanywhere-from-the-gallery"></a><span data-ttu-id="0c430-123">Přidání FilesAnywhere z Galerie</span><span class="sxs-lookup"><span data-stu-id="0c430-123">Adding FilesAnywhere from the gallery</span></span>
<span data-ttu-id="0c430-124">Při konfiguraci integrace FilesAnywhere do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS FilesAnywhere z galerie.</span><span class="sxs-lookup"><span data-stu-id="0c430-124">To configure the integration of FilesAnywhere into Azure AD, you need to add FilesAnywhere from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0c430-125">**Pokud chcete přidat FilesAnywhere z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0c430-125">**To add FilesAnywhere from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0c430-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0c430-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0c430-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0c430-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0c430-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0c430-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0c430-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0c430-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0c430-133">Do vyhledávacího pole zadejte **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="0c430-133">In the search box, type **FilesAnywhere**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_search.png)

5. <span data-ttu-id="0c430-135">Na panelu výsledků vyberte **FilesAnywhere**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c430-135">In the results panel, select **FilesAnywhere**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0c430-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0c430-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0c430-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FilesAnywhere podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="0c430-138">In this section, you configure and test Azure AD single sign-on with FilesAnywhere based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0c430-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v FilesAnywhere je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0c430-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FilesAnywhere is to a user in Azure AD.</span></span> <span data-ttu-id="0c430-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v FilesAnywhere musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0c430-140">In other words, a link relationship between an Azure AD user and the related user in FilesAnywhere needs to be established.</span></span>

<span data-ttu-id="0c430-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="0c430-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FilesAnywhere.</span></span>

<span data-ttu-id="0c430-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s FilesAnywhere, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0c430-142">To configure and test Azure AD single sign-on with FilesAnywhere, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0c430-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0c430-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0c430-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0c430-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0c430-145">**[Vytvoření zkušebního uživatele FilesAnywhere](#creating-a-filesanywhere-test-user)**  – Pokud chcete mít protějšek Britta Simon v FilesAnywhere propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="0c430-145">**[Creating a FilesAnywhere test user](#creating-a-filesanywhere-test-user)** - to have a counterpart of Britta Simon in FilesAnywhere that is linked to the Azure AD representation of her.</span></span>
3. <span data-ttu-id="0c430-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0c430-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
4. <span data-ttu-id="0c430-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0c430-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0c430-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0c430-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0c430-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="0c430-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FilesAnywhere application.</span></span>

<span data-ttu-id="0c430-150">**Ke konfiguraci Azure AD jednotné přihlašování s FilesAnywhere, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0c430-150">**To configure Azure AD single sign-on with FilesAnywhere, perform the following steps:**</span></span>

1. <span data-ttu-id="0c430-151">Na portálu Azure Management portal na **FilesAnywhere** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0c430-151">In the Azure Management portal, on the **FilesAnywhere** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0c430-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="0c430-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

3. <span data-ttu-id="0c430-155">Na **FilesAnywhere domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="0c430-155">On the **FilesAnywhere Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url.png)
    
    <span data-ttu-id="0c430-157">a.</span><span class="sxs-lookup"><span data-stu-id="0c430-157">a.</span></span> <span data-ttu-id="0c430-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span><span class="sxs-lookup"><span data-stu-id="0c430-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span></span>
> [!NOTE]
> <span data-ttu-id="0c430-159">Pamatujte, že hodnota **215** je **clientid** a je jenom jako příklad.</span><span class="sxs-lookup"><span data-stu-id="0c430-159">Please note that the value **215** is a **clientid** and is just an example.</span></span> <span data-ttu-id="0c430-160">Potřebujete nahradit hodnotou skutečné clientid.</span><span class="sxs-lookup"><span data-stu-id="0c430-160">You need to replace it with the actual clientid value.</span></span>

4. <span data-ttu-id="0c430-161">Na **FilesAnywhere domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **SP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0c430-161">On the **FilesAnywhere Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url1.png)

    <span data-ttu-id="0c430-163">a.</span><span class="sxs-lookup"><span data-stu-id="0c430-163">a.</span></span> <span data-ttu-id="0c430-164">Klikněte na **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="0c430-164">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="0c430-165">b.</span><span class="sxs-lookup"><span data-stu-id="0c430-165">b.</span></span> <span data-ttu-id="0c430-166">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<sub domain>.filesanywhere.com/`</span><span class="sxs-lookup"><span data-stu-id="0c430-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<sub domain>.filesanywhere.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0c430-167">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0c430-167">Please note that these are not the real values.</span></span> <span data-ttu-id="0c430-168">Budete muset aktualizovat tyto hodnoty se skutečné přihlašovací adresa URL a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0c430-168">You have to update these values with the actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="0c430-169">Obraťte se na [tým podpory FilesAnywhere](mailto:support@FilesAnywhere.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="0c430-169">Contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) to get these values.</span></span> 

5. <span data-ttu-id="0c430-170">FilesAnywhere softwarová aplikace očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="0c430-170">FilesAnywhere Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="0c430-171">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c430-171">Please configure the following claims for this application.</span></span> <span data-ttu-id="0c430-172">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c430-172">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="0c430-173">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="0c430-173">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    <span data-ttu-id="0c430-175">Pokud se uživatelé přihlásí s FilesAnywhere se získat hodnotu **clientid** atribut z [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span><span class="sxs-lookup"><span data-stu-id="0c430-175">When the users signs up with FilesAnywhere they get the value of **clientid** attribute from [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span></span> <span data-ttu-id="0c430-176">Budete muset přidat atribut "Id klienta" s poskytované FilesAnywhere jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="0c430-176">You have to add the "Client Id" attribute with the unique value provided by FilesAnywhere.</span></span> <span data-ttu-id="0c430-177">Všechny tyto atributy, které jsou uvedené výše se vyžadují.</span><span class="sxs-lookup"><span data-stu-id="0c430-177">All these attributes shown above are required.</span></span>
    > [!NOTE] 
    > <span data-ttu-id="0c430-178">Pamatujte, že hodnota **2331** z **clientid** je jenom jako příklad.</span><span class="sxs-lookup"><span data-stu-id="0c430-178">Please note that the value **2331** of **clientid** is just an example.</span></span> <span data-ttu-id="0c430-179">Je třeba zadat skutečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0c430-179">You need to provide the actual value.</span></span>


6. <span data-ttu-id="0c430-180">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0c430-180">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="0c430-181">Název atributu</span><span class="sxs-lookup"><span data-stu-id="0c430-181">Attribute Name</span></span> | <span data-ttu-id="0c430-182">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="0c430-182">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="0c430-183">ClientID</span><span class="sxs-lookup"><span data-stu-id="0c430-183">clientid</span></span> | <span data-ttu-id="0c430-184">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="0c430-184">*"uniquevalue"*</span></span> |

    <span data-ttu-id="0c430-185">a.</span><span class="sxs-lookup"><span data-stu-id="0c430-185">a.</span></span> <span data-ttu-id="0c430-186">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0c430-186">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    <span data-ttu-id="0c430-189">b.</span><span class="sxs-lookup"><span data-stu-id="0c430-189">b.</span></span> <span data-ttu-id="0c430-190">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="0c430-190">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="0c430-191">c.</span><span class="sxs-lookup"><span data-stu-id="0c430-191">c.</span></span> <span data-ttu-id="0c430-192">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="0c430-192">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="0c430-193">d.</span><span class="sxs-lookup"><span data-stu-id="0c430-193">d.</span></span> <span data-ttu-id="0c430-194">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="0c430-194">Click **Ok**</span></span>

7. <span data-ttu-id="0c430-195">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0c430-195">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="0c430-197">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="0c430-197">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

9. <span data-ttu-id="0c430-199">Na **FilesAnywhere konfigurace** klikněte na tlačítko **konfigurace FilesAnywhere** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0c430-199">On the **FilesAnywhere Configuration** section, click **Configure FilesAnywhere** to open **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

10. <span data-ttu-id="0c430-202">Chcete-li získat dokončení konfigurace jednotného přihlašování pro vaši aplikaci na konci FilesAnywhere, obraťte se na [tým podpory FilesAnywhere](mailto:support@FilesAnywhere.com) a poskytněte stažené tokenu SAML podpisový certifikát a jednotné přihlašování na (SSO) adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0c430-202">To get SSO configuration complete for your application at FilesAnywhere end, contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) and provide them the downloaded SAML token signing Certificate and Single Sign On (SSO) URL.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0c430-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0c430-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="0c430-204">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0c430-204">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0c430-206">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0c430-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0c430-207">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0c430-207">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0c430-209">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0c430-209">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0c430-211">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0c430-211">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0c430-213">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0c430-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0c430-215">a.</span><span class="sxs-lookup"><span data-stu-id="0c430-215">a.</span></span> <span data-ttu-id="0c430-216">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0c430-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0c430-217">b.</span><span class="sxs-lookup"><span data-stu-id="0c430-217">b.</span></span> <span data-ttu-id="0c430-218">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0c430-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0c430-219">c.</span><span class="sxs-lookup"><span data-stu-id="0c430-219">c.</span></span> <span data-ttu-id="0c430-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0c430-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0c430-221">d.</span><span class="sxs-lookup"><span data-stu-id="0c430-221">d.</span></span> <span data-ttu-id="0c430-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0c430-222">Click **Create**.</span></span> 



### <a name="creating-a-filesanywhere-test-user"></a><span data-ttu-id="0c430-223">Vytvoření zkušebního uživatele FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="0c430-223">Creating a FilesAnywhere test user</span></span>

<span data-ttu-id="0c430-224">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele automaticky se vytvoří v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c430-224">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0c430-225">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0c430-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0c430-226">V této části povolíte Britta Simon používat tak, že udělíte přístup k FilesAnywhere Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0c430-226">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to FilesAnywhere.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0c430-228">**Pokud chcete přiřadit Britta Simon FilesAnywhere, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0c430-228">**To assign Britta Simon to FilesAnywhere, perform the following steps:**</span></span>

1. <span data-ttu-id="0c430-229">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0c430-229">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0c430-231">V seznamu aplikací vyberte **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="0c430-231">In the applications list, select **FilesAnywhere**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_app.png) 

3. <span data-ttu-id="0c430-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0c430-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0c430-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0c430-235">Click **Add** button.</span></span> <span data-ttu-id="0c430-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0c430-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0c430-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0c430-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0c430-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0c430-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0c430-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0c430-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="0c430-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0c430-241">Testing single sign-on</span></span>

<span data-ttu-id="0c430-242">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0c430-242">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0c430-243">Když kliknete na dlaždici FilesAnywhere na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="0c430-243">When you click the FilesAnywhere tile in the Access Panel, you should get automatically signed-on to your FilesAnywhere application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="0c430-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0c430-244">Additional resources</span></span>

* [<span data-ttu-id="0c430-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0c430-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0c430-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0c430-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_203.png
