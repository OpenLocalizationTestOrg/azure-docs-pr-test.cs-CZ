---
title: "Autorizovat vývojářským účtům pomocí Azure Active Directory – Azure API Management | Microsoft Docs"
description: "Naučte se autorizace uživatelů pomocí služby Azure Active Directory ve službě API Management."
services: api-management
documentationcenter: API Management
author: steved0x
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 7637e6419d17a2d75904fbe63df5f27d4be4bbe3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a><span data-ttu-id="6e822-103">Jak se mají autorizovat vývojářským účtům pomocí služby Azure Active Directory v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="6e822-103">How to authorize developer accounts using Azure Active Directory in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="6e822-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6e822-104">Overview</span></span>
<span data-ttu-id="6e822-105">Tento průvodce vám ukáže, jak povolit přístup k portálu pro vývojáře pro uživatele ze služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e822-105">This guide shows you how to enable access to the developer portal for users from Azure Active Directory.</span></span> <span data-ttu-id="6e822-106">Tento průvodce také ukazuje, jak spravovat skupiny uživatelů Azure Active Directory tak, že přidáte externí skupiny, které obsahují uživatele, služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e822-106">This guide also shows you how to manage groups of Azure Active Directory users by adding external groups that contain the users of an Azure Active Directory.</span></span>

> <span data-ttu-id="6e822-107">Pokud chcete provést kroky v této příručce musí mít nejprve Azure Active Directory, do kterého chcete vytvořit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e822-107">To complete the steps in this guide you must first have an Azure Active Directory in which to create an application.</span></span>
> 
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a><span data-ttu-id="6e822-108">Jak se mají autorizovat vývojářským účtům pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e822-108">How to authorize developer accounts using Azure Active Directory</span></span>
<span data-ttu-id="6e822-109">Chcete-li začít, klikněte na tlačítko **portál vydavatele** na portálu Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="6e822-109">To get started, click **Publisher portal** in the Azure portal for your API Management service.</span></span> <span data-ttu-id="6e822-110">Tím přejdete na portál vydavatele služby API Management.</span><span class="sxs-lookup"><span data-stu-id="6e822-110">This takes you to the API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="6e822-112">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="6e822-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="6e822-113">Klikněte na tlačítko **zabezpečení** z **API Management** nabídky na levé straně a klikněte na **externí identity**.</span><span class="sxs-lookup"><span data-stu-id="6e822-113">Click **Security** from the **API Management** menu on the left and click **External Identities**.</span></span>

![Externí identity][api-management-security-external-identities]

<span data-ttu-id="6e822-115">Klikněte na tlačítko **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e822-115">Click **Azure Active Directory**.</span></span> <span data-ttu-id="6e822-116">Poznamenejte si **adresy URL pro přesměrování** a přejít do služby Azure Active Directory na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="6e822-116">Make a note of the **Redirect URL** and switch over to your Azure Active Directory in the Azure Classic Portal.</span></span>

![Externí identity][api-management-security-aad-new]

<span data-ttu-id="6e822-118">Klikněte **přidat** tlačítko Vytvořit novou aplikaci Azure Active Directory a vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="6e822-118">Click the **Add** button to create a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Přidejte novou aplikaci Azure Active Directory][api-management-new-aad-application-menu]

<span data-ttu-id="6e822-120">Zadejte název aplikace, vyberte možnost **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="6e822-120">Enter a name for the application, select **Web application and/or Web API**, and click the next button.</span></span>

![Novou aplikaci Azure Active Directory][api-management-new-aad-application-1]

<span data-ttu-id="6e822-122">Pro **přihlašovací adresa URL**, zadejte adresu URL přihlašování portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="6e822-122">For **Sign-on URL**, enter the sign-on URL of your developer portal.</span></span> <span data-ttu-id="6e822-123">V tomto příkladu **přihlašovací adresa URL** je `https://aad03.portal.current.int-azure-api.net/signin`.</span><span class="sxs-lookup"><span data-stu-id="6e822-123">In this example, the **Sign-on URL** is `https://aad03.portal.current.int-azure-api.net/signin`.</span></span> 

<span data-ttu-id="6e822-124">Pro **URL ID aplikace**, zadejte buď výchozí nebo vlastní doménu pro Azure Active Directory a k němu připojí do jedinečného řetězce.</span><span class="sxs-lookup"><span data-stu-id="6e822-124">For the **App ID URL**, enter either the default domain or a custom domain for the Azure Active Directory, and append a unique string to it.</span></span> <span data-ttu-id="6e822-125">V tomto příkladu výchozí doménu **https://contoso5api.onmicrosoft.com** se používá s příponou **/api** zadaný.</span><span class="sxs-lookup"><span data-stu-id="6e822-125">In this example, the default domain of **https://contoso5api.onmicrosoft.com** is used with the suffix of **/api** specified.</span></span>

![Nové vlastnosti aplikace Azure Active Directory][api-management-new-aad-application-2]

<span data-ttu-id="6e822-127">Klikněte na tlačítko zaškrtnutí a uložte a vytvořit aplikaci, přepněte na **konfigurace** kartu a nakonfigurujte novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e822-127">Click the check button to save and create the application, and switch to the **Configure** tab to configure the new application.</span></span>

![Vytvořit novou aplikaci Azure Active Directory][api-management-new-aad-app-created]

<span data-ttu-id="6e822-129">Pokud se má použít pro tuto aplikaci více Azure Active Directory, klikněte na tlačítko **Ano** pro **aplikace je víceklientské**.</span><span class="sxs-lookup"><span data-stu-id="6e822-129">If multiple Azure Active Directories are going to be used for this application, click **Yes** for **Application is multi-tenant**.</span></span> <span data-ttu-id="6e822-130">Výchozí hodnota je **ne**.</span><span class="sxs-lookup"><span data-stu-id="6e822-130">The default is **No**.</span></span>

![Aplikace je více klientů][api-management-aad-app-multi-tenant]

<span data-ttu-id="6e822-132">Kopírování **adresy URL pro přesměrování** z **Azure Active Directory** části **externí identity** kartě na portálu vydavatele a vložte ji do **odpovědi Adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6e822-132">Copy the **Redirect URL** from the **Azure Active Directory** section of the **External Identities** tab in the publisher portal and paste it into the **Reply URL** text box.</span></span> 

![Adresa URL odpovědi][api-management-aad-reply-url]

<span data-ttu-id="6e822-134">Přejděte do dolní části kartu Konfigurace, vyberte **oprávnění aplikací** rozevíracího seznamu a zkontrolujte **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="6e822-134">Scroll to the bottom of the configure tab, select the **Application Permissions** drop-down, and check **Read directory data**.</span></span>

![Oprávnění aplikací][api-management-aad-app-permissions]

<span data-ttu-id="6e822-136">Vyberte **delegování oprávnění** rozevíracího seznamu a zkontrolujte **povolit přihlášení a čtení uživatelských profilů**.</span><span class="sxs-lookup"><span data-stu-id="6e822-136">Select the **Delegate Permissions** drop-down, and check **Enable sign-on and read users' profiles**.</span></span>

![Přidělená oprávnění][api-management-aad-delegated-permissions]

> <span data-ttu-id="6e822-138">Další informace o aplikaci a přidělená oprávnění najdete v tématu [přístup k rozhraní Graph API][Accessing the Graph API].</span><span class="sxs-lookup"><span data-stu-id="6e822-138">For more information about application and delegated permissions, see [Accessing the Graph API][Accessing the Graph API].</span></span>
> 
> 

<span data-ttu-id="6e822-139">Kopírování **Id klienta** do schránky.</span><span class="sxs-lookup"><span data-stu-id="6e822-139">Copy the **Client Id** to the clipboard.</span></span>

![Id klienta][api-management-aad-app-client-id]

<span data-ttu-id="6e822-141">Přepněte zpět na portál vydavatele a vložte **Id klienta** zkopírovaných z konfigurace aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e822-141">Switch back to the publisher portal and paste in the **Client Id** copied from the Azure Active Directory application configuration.</span></span>

![Id klienta][api-management-client-id]

<span data-ttu-id="6e822-143">Přepněte zpět na konfiguraci služby Azure Active Directory a klikněte na tlačítko **vyberte dobu trvání** rozevírací seznam v **klíče** části a zadat interval.</span><span class="sxs-lookup"><span data-stu-id="6e822-143">Switch back to the Azure Active Directory configuration, and click the **Select duration** drop-down in the **Keys** section and specify an interval.</span></span> <span data-ttu-id="6e822-144">V tomto příkladu **1 rok** se používá.</span><span class="sxs-lookup"><span data-stu-id="6e822-144">In this example, **1 year** is used.</span></span>

![Klíč][api-management-aad-key-before-save]

<span data-ttu-id="6e822-146">Klikněte na tlačítko **Uložit** zobrazí klíč a uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6e822-146">Click **Save** to save the configuration and display the key.</span></span> <span data-ttu-id="6e822-147">Klíč zkopírujte do schránky.</span><span class="sxs-lookup"><span data-stu-id="6e822-147">Copy the key to the clipboard.</span></span>

> <span data-ttu-id="6e822-148">Tento klíč si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="6e822-148">Make a note of this key.</span></span> <span data-ttu-id="6e822-149">Po zavření okna konfigurace Azure Active Directory, klíč nelze zobrazit znovu.</span><span class="sxs-lookup"><span data-stu-id="6e822-149">Once you close the Azure Active Directory configuration window, the key cannot be displayed again.</span></span>
> 
> 

![Klíč][api-management-aad-key-after-save]

<span data-ttu-id="6e822-151">Přepněte zpět na portál vydavatele a vložte klíč do **tajný klíč klienta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6e822-151">Switch back to the publisher portal and paste the key into the **Client Secret** text box.</span></span>

![Tajný klíč klienta][api-management-client-secret]

<span data-ttu-id="6e822-153">**Povolené klienty** Určuje adresáře, které mají přístup k rozhraním API v instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="6e822-153">**Allowed Tenants** specifies which directories have access to the APIs of the API Management service instance.</span></span> <span data-ttu-id="6e822-154">Zadejte domény instancí Azure Active Directory, ke kterým chcete udělit přístup.</span><span class="sxs-lookup"><span data-stu-id="6e822-154">Specify the domains of the Azure Active Directory instances to which you want to grant access.</span></span> <span data-ttu-id="6e822-155">Více domén můžete oddělit vložení znaků newline, mezery nebo čárkami.</span><span class="sxs-lookup"><span data-stu-id="6e822-155">You can separate multiple domains with newlines, spaces, or commas.</span></span>

![Povolené klientů][api-management-client-allowed-tenants]


<span data-ttu-id="6e822-157">Jakmile je zadána požadované konfigurace, klikněte na možnost **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6e822-157">Once the desired configuration is specified, click **Save**.</span></span>

![Uložit][api-management-client-allowed-tenants-save]

<span data-ttu-id="6e822-159">Po změny jsou uloženy, uživatelé v zadané Azure Active Directory můžete přihlásit na portál pro vývojáře podle kroků v [Přihlaste se k portálu pro vývojáře pomocí účtu Azure Active Directory] [ Log in to the Developer portal using an Azure Active Directory account].</span><span class="sxs-lookup"><span data-stu-id="6e822-159">Once the changes are saved, the users in the specified Azure Active Directory can sign in to the Developer portal by following the steps in [Log in to the Developer portal using an Azure Active Directory account][Log in to the Developer portal using an Azure Active Directory account].</span></span>

<span data-ttu-id="6e822-160">V lze zadat více domén **klientům povoleno** části.</span><span class="sxs-lookup"><span data-stu-id="6e822-160">Multiple domains can be specified in the **Allowed Tenants** section.</span></span> <span data-ttu-id="6e822-161">Než každý uživatel může přihlásit z jiné doméně než původní domény, kde byla aplikace registrovaná, globální správce jinou doménu, musí udělit oprávnění pro aplikaci pro přístup k datům adresáře.</span><span class="sxs-lookup"><span data-stu-id="6e822-161">Before any user can log in from a different domain than the original domain where the application was registered, a global administrator of the different domain must grant permission for the application to access directory data.</span></span> <span data-ttu-id="6e822-162">Udělit oprávnění, má globální správce, přejděte k `https://<URL of your developer portal>/aadadminconsent` (například https://contoso.portal.azure-api.net/aadadminconsent), zadejte název domény klienta služby Active Directory chtějí poskytnout přístup a klikněte na tlačítko Odeslat.</span><span class="sxs-lookup"><span data-stu-id="6e822-162">To grant permission, the global administrator should go to `https://<URL of your developer portal>/aadadminconsent` (for example, https://contoso.portal.azure-api.net/aadadminconsent), type in the domain name of the Active Directory tenant they want to give access to and click Submit.</span></span> <span data-ttu-id="6e822-163">V následujícím příkladu, globální správce `miaoaad.onmicrosoft.com` pokouší udělit oprávnění k tomuto portálu konkrétní developer.</span><span class="sxs-lookup"><span data-stu-id="6e822-163">In the following example, a global administrator from `miaoaad.onmicrosoft.com` is trying to give permission to this particular developer portal.</span></span> 

![Oprávnění][api-management-aad-consent]

<span data-ttu-id="6e822-165">Na další obrazovce se výzva k potvrzení, udělíte oprávnění globálního správce.</span><span class="sxs-lookup"><span data-stu-id="6e822-165">In the next screen, the global administrator will be prompted to confirm giving the permission.</span></span> 

![Oprávnění][api-management-permissions-form]

> <span data-ttu-id="6e822-167">Pokud není globální správce pokusí o přihlášení, než oprávnění globální správce, pokus o přihlášení selže a se zobrazí chybové obrazovce.</span><span class="sxs-lookup"><span data-stu-id="6e822-167">If a non-global administrator tries to log in before permissions are granted by a global administrator, the login attempt fails and an error screen is displayed.</span></span>
> 
> 

## <a name="how-to-add-an-external-azure-active-directory-group"></a><span data-ttu-id="6e822-168">Postup přidání externí Azure skupině služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e822-168">How to add an external Azure Active Directory Group</span></span>
<span data-ttu-id="6e822-169">Když povolíte přístup pro uživatele v Azure Active Directory, můžete přidat skupiny Azure Active Directory do rozhraní API správy snadnější správu přidružení požadované produkty vývojářů ve skupině.</span><span class="sxs-lookup"><span data-stu-id="6e822-169">After enabling access for users in an Azure Active Directory, you can add Azure Active Directory groups into API Management to more easily manage the association of the developers in the group with the desired products.</span></span>

> <span data-ttu-id="6e822-170">Ke konfiguraci externích skupin Azure Active Directory, Azure Active Directory musí nejdřív nakonfigurovat na kartě identit pomocí postupu v předchozím oddílu.</span><span class="sxs-lookup"><span data-stu-id="6e822-170">To configure an external Azure Active Directory group, the Azure Active Directory must first be configured in the Identities tab by following the procedure in the previous section.</span></span> 
> 
> 

<span data-ttu-id="6e822-171">Externí skupiny Azure Active Directory se přidají **viditelnost** karta produktu, pro které chcete udělit přístup ke skupině.</span><span class="sxs-lookup"><span data-stu-id="6e822-171">External Azure Active Directory groups are added from the **Visibility** tab of the product for which you wish to grant access to the group.</span></span> <span data-ttu-id="6e822-172">Klikněte na tlačítko **produkty**a potom klikněte na název požadovaného produktu.</span><span class="sxs-lookup"><span data-stu-id="6e822-172">Click **Products**, and then click the name of the desired product.</span></span>

![Konfigurace produktu][api-management-configure-product]

<span data-ttu-id="6e822-174">Přepnout **viditelnost** a klikněte na **přidat skupiny z Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e822-174">Switch to the **Visibility** tab, and click **Add Groups from Azure Active Directory**.</span></span>

![Přidání skupin][api-management-add-groups]

<span data-ttu-id="6e822-176">Vyberte **klienta Azure Active Directory** z rozevíracího seznamu a pak zadejte název požadované skupiny v **skupiny** přidat textové pole.</span><span class="sxs-lookup"><span data-stu-id="6e822-176">Select the **Azure Active Directory Tenant** from the drop-down list, and then type the name of the desired group in the **Groups** to be added text box.</span></span>

![Vyberte skupinu][api-management-select-group]

<span data-ttu-id="6e822-178">Tento název skupiny lze nalézt v **skupiny** seznam pro Azure Active Directory, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="6e822-178">This group name can be found in the **Groups** list for your Azure Active Directory, as shown in the following example.</span></span>

![Seznam skupin Azure Active Directory][api-management-aad-groups-list]

<span data-ttu-id="6e822-180">Klikněte na tlačítko **přidat** ověřit název skupiny a přidejte skupinu.</span><span class="sxs-lookup"><span data-stu-id="6e822-180">Click **Add** to validate the group name and add the group.</span></span> <span data-ttu-id="6e822-181">V tomto příkladu **Contoso 5 vývojáři** je přidána externí skupina.</span><span class="sxs-lookup"><span data-stu-id="6e822-181">In this example, the **Contoso 5 Developers** external group is added.</span></span> 

![Přidat skupinu][api-management-aad-group-added]

<span data-ttu-id="6e822-183">Klikněte na tlačítko **Uložit** uložte novou skupinu výběr.</span><span class="sxs-lookup"><span data-stu-id="6e822-183">Click **Save** to save the new group selection.</span></span>

<span data-ttu-id="6e822-184">Jakmile skupinu služby Azure Active Directory byla nakonfigurována jedním z produktů, je k dispozici kontrolu **viditelnost** kartě pro ostatní produkty v instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="6e822-184">Once an Azure Active Directory group has been configured from one product, it is available to be checked on the **Visibility** tab for the other products in the API Management service instance.</span></span>

<span data-ttu-id="6e822-185">Zkontrolujte a konfigurujte vlastnosti externí skupiny po byly přidány, klikněte na název skupiny z **skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="6e822-185">To review and configure the properties for external groups once they have been added, click the name of the group from the **Groups** tab.</span></span>

![Správa skupin][api-management-groups]

<span data-ttu-id="6e822-187">Zde můžete upravit **název** a **popis** skupiny.</span><span class="sxs-lookup"><span data-stu-id="6e822-187">From here you can edit the **Name** and the **Description** of the group.</span></span>

![Úprava skupiny][api-management-edit-group]

<span data-ttu-id="6e822-189">Uživatelé z nakonfigurovaných Azure Active Directory můžete přihlásit k portálu pro vývojáře a zobrazení a přihlášení k odběru do žádné skupiny, ke kterým mají viditelnost podle pokynů uvedených v následující části.</span><span class="sxs-lookup"><span data-stu-id="6e822-189">Users from the configured Azure Active Directory can sign in to the Developer portal and view and subscribe to any groups for which they have visibility by following the instructions in the following section.</span></span>

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><span data-ttu-id="6e822-190">Jak se přihlásit na portál pro vývojáře pomocí účtu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e822-190">How to log in to the Developer portal using an Azure Active Directory account</span></span>
<span data-ttu-id="6e822-191">Přihlaste se pomocí účtu Azure Active Directory nakonfigurované v předchozích částech portálu pro vývojáře, otevřete nové okno prohlížeče pomocí **přihlašovací adresa URL** z konfigurace aplikace služby Active Directory a klikněte na tlačítko **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e822-191">To log into the Developer portal using an Azure Active Directory account configured in the previous sections, open a new browser window using the **Sign-on URL** from the Active Directory application configuration, and click **Azure Active Directory**.</span></span>

![Portál pro vývojáře][api-management-dev-portal-signin]

<span data-ttu-id="6e822-193">Zadejte přihlašovací údaje pro jeden z uživatelů ve vašem Azure Active Directory a klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="6e822-193">Enter the credentials of one of the users in your Azure Active Directory, and click **Sign in**.</span></span>

![Přihlášení][api-management-aad-signin]

<span data-ttu-id="6e822-195">S registračním formuláři můžete být vyzváni, pokud je vyžadováno žádné další údaje.</span><span class="sxs-lookup"><span data-stu-id="6e822-195">You may be prompted with a registration form if any additional information is required.</span></span> <span data-ttu-id="6e822-196">Dokončení registrace a klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="6e822-196">Complete the registration form and click **Sign up**.</span></span>

![Registrace][api-management-complete-registration]

<span data-ttu-id="6e822-198">Uživatel je nyní přihlášeni portál pro vývojáře pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="6e822-198">Your user is now logged into the developer portal for your API Management service instance.</span></span>

![Registrace je dokončena.][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

