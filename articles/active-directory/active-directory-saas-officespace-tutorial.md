---
title: 'Kurz: Azure Active Directory integrace s OfficeSpace softwarem | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi OfficeSpace softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 43d2ecfe851d8f6c43cd4ce7fc4bd872818f4137
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a><span data-ttu-id="6bc13-103">Kurz: Azure Active Directory integrace s OfficeSpace softwaru</span><span class="sxs-lookup"><span data-stu-id="6bc13-103">Tutorial: Azure Active Directory integration with OfficeSpace Software</span></span>

<span data-ttu-id="6bc13-104">V tomto kurzu zjistěte, jak integrovat OfficeSpace softwaru s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6bc13-104">In this tutorial, you learn how to integrate OfficeSpace Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6bc13-105">Integrace OfficeSpace softwaru s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6bc13-105">Integrating OfficeSpace Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6bc13-106">Můžete ovládat ve službě Azure AD, který má přístup k OfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="6bc13-106">You can control in Azure AD who has access to OfficeSpace Software.</span></span>
- <span data-ttu-id="6bc13-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k softwaru OfficeSpace (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6bc13-107">You can enable your users to automatically get signed-on to OfficeSpace Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6bc13-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6bc13-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="6bc13-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6bc13-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6bc13-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6bc13-110">Prerequisites</span></span>

<span data-ttu-id="6bc13-111">Konfigurace integrace Azure AD s OfficeSpace softwaru, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="6bc13-111">To configure Azure AD integration with OfficeSpace Software, you need the following items:</span></span>

- <span data-ttu-id="6bc13-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6bc13-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6bc13-113">OfficeSpace softwaru jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6bc13-113">A OfficeSpace Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6bc13-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6bc13-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6bc13-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6bc13-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6bc13-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6bc13-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6bc13-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6bc13-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6bc13-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6bc13-118">Scenario description</span></span>
<span data-ttu-id="6bc13-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6bc13-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6bc13-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6bc13-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6bc13-121">Přidání softwaru OfficeSpace z Galerie</span><span class="sxs-lookup"><span data-stu-id="6bc13-121">Adding OfficeSpace Software from the gallery</span></span>
2. <span data-ttu-id="6bc13-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bc13-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-officespace-software-from-the-gallery"></a><span data-ttu-id="6bc13-123">Přidání softwaru OfficeSpace z Galerie</span><span class="sxs-lookup"><span data-stu-id="6bc13-123">Adding OfficeSpace Software from the gallery</span></span>
<span data-ttu-id="6bc13-124">Při konfiguraci integrace OfficeSpace softwaru do služby Azure AD, musíte přidat OfficeSpace Software z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6bc13-124">To configure the integration of OfficeSpace Software into Azure AD, you need to add OfficeSpace Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6bc13-125">**Pokud chcete přidat OfficeSpace softwaru z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bc13-125">**To add OfficeSpace Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6bc13-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6bc13-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="6bc13-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6bc13-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="6bc13-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bc13-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="6bc13-133">Do vyhledávacího pole zadejte **OfficeSpace softwaru**, vyberte **OfficeSpace softwaru** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6bc13-133">In the search box, type **OfficeSpace Software**, select **OfficeSpace Software** from result panel then click **Add** button to add the application.</span></span>

    ![OfficeSpace softwaru v seznamu výsledků](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6bc13-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bc13-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6bc13-136">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s OfficeSpace softwarem podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6bc13-136">In this section, you configure and test Azure AD single sign-on with OfficeSpace Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6bc13-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v OfficeSpace softwaru je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6bc13-137">For single sign-on to work, Azure AD needs to know what the counterpart user in OfficeSpace Software is to a user in Azure AD.</span></span> <span data-ttu-id="6bc13-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v OfficeSpace softwaru je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="6bc13-138">In other words, a link relationship between an Azure AD user and the related user in OfficeSpace Software needs to be established.</span></span>

<span data-ttu-id="6bc13-139">V softwaru OfficeSpace přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="6bc13-139">In OfficeSpace Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6bc13-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s OfficeSpace softwaru, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="6bc13-140">To configure and test Azure AD single sign-on with OfficeSpace Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6bc13-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="6bc13-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6bc13-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bc13-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6bc13-143">**[Vytvoření zkušebního uživatele softwaru OfficeSpace](#create-a-officespace-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v OfficeSpace Software, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6bc13-143">**[Create a OfficeSpace Software test user](#create-a-officespace-software-test-user)** - to have a counterpart of Britta Simon in OfficeSpace Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6bc13-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6bc13-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6bc13-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6bc13-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6bc13-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bc13-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6bc13-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci OfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="6bc13-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your OfficeSpace Software application.</span></span>

<span data-ttu-id="6bc13-148">**Ke konfiguraci Azure AD jednotné přihlašování s OfficeSpace softwarem, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bc13-148">**To configure Azure AD single sign-on with OfficeSpace Software, perform the following steps:**</span></span>

1. <span data-ttu-id="6bc13-149">Na portálu Azure na **OfficeSpace softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-149">In the Azure portal, on the **OfficeSpace Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="6bc13-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6bc13-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. <span data-ttu-id="6bc13-153">Na **OfficeSpace softwaru domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bc13-153">On the **OfficeSpace Software Domain and URLs** section, perform the following steps:</span></span>

    ![OfficeSpace softwaru domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    <span data-ttu-id="6bc13-155">a.</span><span class="sxs-lookup"><span data-stu-id="6bc13-155">a.</span></span> <span data-ttu-id="6bc13-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.officespacesoftware.com/users/sign_in/saml`</span><span class="sxs-lookup"><span data-stu-id="6bc13-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.officespacesoftware.com/users/sign_in/saml`</span></span>

    <span data-ttu-id="6bc13-157">b.</span><span class="sxs-lookup"><span data-stu-id="6bc13-157">b.</span></span> <span data-ttu-id="6bc13-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`<company name>.officespacesoftware.com`</span><span class="sxs-lookup"><span data-stu-id="6bc13-158">In the **Identifier** textbox, type a URL using the following pattern: `<company name>.officespacesoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6bc13-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6bc13-159">These values are not real.</span></span> <span data-ttu-id="6bc13-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6bc13-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6bc13-161">Obraťte se na [tým podpory klientský Software OfficeSpace](mailto:support@officespacesoftware.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="6bc13-161">Contact [OfficeSpace Software Client support team](mailto:support@officespacesoftware.com) to get these values.</span></span> 

4. <span data-ttu-id="6bc13-162">OfficeSpace softwarová aplikace očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="6bc13-162">OfficeSpace Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="6bc13-163">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6bc13-163">Please configure the following claims for this application.</span></span> <span data-ttu-id="6bc13-164">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="6bc13-164">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="6bc13-165">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="6bc13-165">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurace atributů](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. <span data-ttu-id="6bc13-167">V **uživatelské atributy** části na **jednotného přihlašování** dialogovém okně, vyberte **user.mail** jako **uživatelský identifikátor** a pro každý řádek v tabulce níže, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bc13-167">In the **User Attributes** section on the **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in the table below, perform the following steps:</span></span>
    
    | <span data-ttu-id="6bc13-168">Název atributu</span><span class="sxs-lookup"><span data-stu-id="6bc13-168">Attribute Name</span></span> | <span data-ttu-id="6bc13-169">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="6bc13-169">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="6bc13-170">E-mailu</span><span class="sxs-lookup"><span data-stu-id="6bc13-170">email</span></span> | <span data-ttu-id="6bc13-171">User.Mail</span><span class="sxs-lookup"><span data-stu-id="6bc13-171">user.mail</span></span> |
    | <span data-ttu-id="6bc13-172">jméno</span><span class="sxs-lookup"><span data-stu-id="6bc13-172">name</span></span> | <span data-ttu-id="6bc13-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="6bc13-173">user.displayname</span></span> |
    | <span data-ttu-id="6bc13-174">křestní_jméno</span><span class="sxs-lookup"><span data-stu-id="6bc13-174">first_name</span></span> | <span data-ttu-id="6bc13-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="6bc13-175">user.givenname</span></span> |
    | <span data-ttu-id="6bc13-176">Příjmení</span><span class="sxs-lookup"><span data-stu-id="6bc13-176">last_name</span></span> | <span data-ttu-id="6bc13-177">User.Surname</span><span class="sxs-lookup"><span data-stu-id="6bc13-177">user.surname</span></span> |

    <span data-ttu-id="6bc13-178">a.</span><span class="sxs-lookup"><span data-stu-id="6bc13-178">a.</span></span> <span data-ttu-id="6bc13-179">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bc13-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![<span data-ttu-id="6bc13-180">Konfigurace přidat</span><span class="sxs-lookup"><span data-stu-id="6bc13-180">Configure Add</span></span> ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Konfigurace atributů](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="6bc13-182">b.</span><span class="sxs-lookup"><span data-stu-id="6bc13-182">b.</span></span> <span data-ttu-id="6bc13-183">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="6bc13-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="6bc13-184">c.</span><span class="sxs-lookup"><span data-stu-id="6bc13-184">c.</span></span> <span data-ttu-id="6bc13-185">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="6bc13-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="6bc13-186">d.</span><span class="sxs-lookup"><span data-stu-id="6bc13-186">d.</span></span> <span data-ttu-id="6bc13-187">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="6bc13-187">Click **Ok**</span></span>
 
6. <span data-ttu-id="6bc13-188">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="6bc13-188">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of the certificate.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. <span data-ttu-id="6bc13-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6bc13-190">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6bc13-192">Na **OfficeSpace softwarové konfigurace** klikněte na tlačítko **konfigurace softwaru OfficeSpace** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6bc13-192">On the **OfficeSpace Software Configuration** section, click **Configure OfficeSpace Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6bc13-193">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6bc13-193">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace OfficeSpace softwaru](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. <span data-ttu-id="6bc13-195">V okně prohlížeče jiný web Přihlaste se ke klientovi OfficeSpace softwaru jako správce.</span><span class="sxs-lookup"><span data-stu-id="6bc13-195">In a different web browser window, log into your OfficeSpace Software tenant as an administrator.</span></span>

10. <span data-ttu-id="6bc13-196">Přejděte na **nastavení** a klikněte na tlačítko **konektory**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-196">Go to **Settings** and click **Connectors**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. <span data-ttu-id="6bc13-198">Klikněte na tlačítko **ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-198">Click **SAML Authentication**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. <span data-ttu-id="6bc13-200">V **ověřování SAML** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bc13-200">In the **SAML Authentication** section, perform the following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    <span data-ttu-id="6bc13-202">a.</span><span class="sxs-lookup"><span data-stu-id="6bc13-202">a.</span></span> <span data-ttu-id="6bc13-203">V **adresy url odhlašovací zprostředkovatele** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6bc13-203">In the **Logout provider url** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6bc13-204">b.</span><span class="sxs-lookup"><span data-stu-id="6bc13-204">b.</span></span> <span data-ttu-id="6bc13-205">V **klienta idp cílová adresa url** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6bc13-205">In the **Client idp target url** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6bc13-206">c.</span><span class="sxs-lookup"><span data-stu-id="6bc13-206">c.</span></span> <span data-ttu-id="6bc13-207">Vložení **kryptografický otisk** hodnotu, která jste zkopírovali z portálu Azure do **otisk certifikátu klienta IDP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6bc13-207">Paste the **Thumbprint** value which you have copied from Azure portal, into the **Client IDP certificate fingerprint** textbox.</span></span> 

    <span data-ttu-id="6bc13-208">d.</span><span class="sxs-lookup"><span data-stu-id="6bc13-208">d.</span></span> <span data-ttu-id="6bc13-209">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-209">Click **Save Settings**.</span></span>


> [!TIP]
> <span data-ttu-id="6bc13-210">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="6bc13-210">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6bc13-211">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="6bc13-211">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6bc13-212">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6bc13-212">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6bc13-213">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6bc13-213">Create an Azure AD test user</span></span>

<span data-ttu-id="6bc13-214">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bc13-214">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="6bc13-216">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bc13-216">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6bc13-217">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6bc13-217">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6bc13-219">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-219">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6bc13-221">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bc13-221">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6bc13-223">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6bc13-223">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6bc13-225">a.</span><span class="sxs-lookup"><span data-stu-id="6bc13-225">a.</span></span> <span data-ttu-id="6bc13-226">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-226">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6bc13-227">b.</span><span class="sxs-lookup"><span data-stu-id="6bc13-227">b.</span></span> <span data-ttu-id="6bc13-228">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6bc13-228">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="6bc13-229">c.</span><span class="sxs-lookup"><span data-stu-id="6bc13-229">c.</span></span> <span data-ttu-id="6bc13-230">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="6bc13-230">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="6bc13-231">d.</span><span class="sxs-lookup"><span data-stu-id="6bc13-231">d.</span></span> <span data-ttu-id="6bc13-232">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-232">Click **Create**.</span></span>
 
### <a name="create-a-officespace-software-test-user"></a><span data-ttu-id="6bc13-233">Vytvoření zkušebního uživatele OfficeSpace softwaru</span><span class="sxs-lookup"><span data-stu-id="6bc13-233">Create a OfficeSpace Software test user</span></span>

<span data-ttu-id="6bc13-234">Cílem této části je vytvoření uživatele volal Britta Simon v OfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="6bc13-234">The objective of this section is to create a user called Britta Simon in OfficeSpace Software.</span></span> <span data-ttu-id="6bc13-235">OfficeSpace Software podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="6bc13-235">OfficeSpace Software supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="6bc13-236">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="6bc13-236">There is no action item for you in this section.</span></span> <span data-ttu-id="6bc13-237">Vytvoří se nový uživatel během pokusu o přístup k softwaru OfficeSpace, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="6bc13-237">A new user will be created during an attempt to access OfficeSpace Software if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="6bc13-238">Pokud potřebujete vytvořit uživatele s ručně, musíte kontaktovat [tým podpory OfficeSpace softwaru](mailto:support@officespacesoftware.com).</span><span class="sxs-lookup"><span data-stu-id="6bc13-238">If you need to create an user manually, you need to Contact [OfficeSpace Software support team](mailto:support@officespacesoftware.com).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="6bc13-239">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6bc13-239">Assign the Azure AD test user</span></span>

<span data-ttu-id="6bc13-240">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu OfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="6bc13-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to OfficeSpace Software.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="6bc13-242">**Pokud chcete přiřadit Britta Simon OfficeSpace softwaru, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6bc13-242">**To assign Britta Simon to OfficeSpace Software, perform the following steps:**</span></span>

1. <span data-ttu-id="6bc13-243">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6bc13-245">V seznamu aplikací vyberte **OfficeSpace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-245">In the applications list, select **OfficeSpace Software**.</span></span>

    ![V seznamu aplikací na odkaz OfficeSpace softwaru](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. <span data-ttu-id="6bc13-247">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6bc13-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="6bc13-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6bc13-249">Click **Add** button.</span></span> <span data-ttu-id="6bc13-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bc13-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="6bc13-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6bc13-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6bc13-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bc13-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6bc13-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6bc13-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6bc13-255">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6bc13-255">Test single sign-on</span></span>

<span data-ttu-id="6bc13-256">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6bc13-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6bc13-257">Když kliknete na dlaždici OfficeSpace softwaru na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci OfficeSpace softwaru.</span><span class="sxs-lookup"><span data-stu-id="6bc13-257">When you click the OfficeSpace Software tile in the Access Panel, you should get automatically signed-on to your OfficeSpace Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6bc13-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6bc13-258">Additional resources</span></span>

* [<span data-ttu-id="6bc13-259">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6bc13-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6bc13-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6bc13-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

