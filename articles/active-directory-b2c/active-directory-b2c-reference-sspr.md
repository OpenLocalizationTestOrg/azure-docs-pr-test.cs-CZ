---
title: "Azure Active Directory B2C: Resetování hesla pomocí samoobslužné služby | Microsoft Docs"
description: "Téma ukázka, jak resetování tooset si hesla pomocí samoobslužné služby pro uživatele v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: ff87eea42259b610702da73af35d0a38eb7dd33d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a><span data-ttu-id="e8d84-103">Azure Active Directory B2C: Nastavení samoobslužného resetování hesla pro uživatele</span><span class="sxs-lookup"><span data-stu-id="e8d84-103">Azure Active Directory B2C: Set up self-service password reset for your consumers</span></span>
<span data-ttu-id="e8d84-104">S hello samoobslužného resetování hesel funkce, uživatele (kteří zaregistrovali pro místní účty) mohou resetovat svá hesla na své vlastní.</span><span class="sxs-lookup"><span data-stu-id="e8d84-104">With hello self-service password reset feature, your consumers (who have signed up for local accounts) can reset their passwords on their own.</span></span> <span data-ttu-id="e8d84-105">To významně snižuje zatížení hello vašim zaměstnancům technické podpory, zvlášť pokud vaše aplikace nemá milionům příjemcům na základě v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="e8d84-105">This significantly reduces hello burden on your support staff, especially if your application has millions of consumers using it on a regular basis.</span></span> <span data-ttu-id="e8d84-106">V současné době podporujeme jenom pomocí ověřenou e-mailovou adresu jako metodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="e8d84-106">Currently, we only support using a verified email address as a recovery method.</span></span> <span data-ttu-id="e8d84-107">V budoucnu hello přidáme obnovení dodatečné metody (ověřené telefonní číslo, bezpečnostní otázky atd.).</span><span class="sxs-lookup"><span data-stu-id="e8d84-107">We will add additional recovery methods (verified phone number, security questions, etc.) in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="e8d84-108">Tento článek se týká tooself služby heslo resetovat použít v kontextu hello zásady přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e8d84-108">This article applies tooself-service password reset used in hello context of a sign-in policy.</span></span> <span data-ttu-id="e8d84-109">Pokud potřebujete zásady resetování plně přizpůsobitelná hesel volána z vaší aplikace, najdete v části [v tomto článku](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="e8d84-109">If you need fully customizable password reset policies invoked from your app, see [this article](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span></span>
> 
> 

<span data-ttu-id="e8d84-110">Ve výchozím nastavení, nebudou mít adresáře hesla pomocí samoobslužné služby resetování zapnutý.</span><span class="sxs-lookup"><span data-stu-id="e8d84-110">By default, your directory will not have self-service password reset turned on.</span></span> <span data-ttu-id="e8d84-111">Použití hello následující kroky tooturn ho na:</span><span class="sxs-lookup"><span data-stu-id="e8d84-111">Use hello following steps tooturn it on:</span></span>

1. <span data-ttu-id="e8d84-112">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) jako hello správce předplatného.</span><span class="sxs-lookup"><span data-stu-id="e8d84-112">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) as hello Subscription Administrator.</span></span> <span data-ttu-id="e8d84-113">Toto je stejný pracovní nebo školní účet nebo hello stejný účet Microsoft, kterou jste použili toocreate adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="e8d84-113">This is hello same work or school account or hello same Microsoft account that you used toocreate your directory.</span></span>
2. <span data-ttu-id="e8d84-114">Přejděte toohello rozšíření Active Directory na hello navigačním panelu na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="e8d84-114">Navigate toohello Active Directory extension on hello navigation bar on hello left side.</span></span>
3. <span data-ttu-id="e8d84-115">Najít adresáře v rámci hello **Directory** kartě a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="e8d84-115">Find your directory under hello **Directory** tab and click it.</span></span>
4. <span data-ttu-id="e8d84-116">Klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="e8d84-116">Click hello **Configure** tab.</span></span>
5. <span data-ttu-id="e8d84-117">Projděte dolů toohello **zásady resetování hesel uživatelů** části a přepínání hello **uživatele povolen pro resetování hesla** možnost příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="e8d84-117">Scroll down toohello **User password reset policy** section and toggle hello **Users enabled for password reset** option too**YES**.</span></span> <span data-ttu-id="e8d84-118">Všimněte si, že hello **alternativní e-mailovou adresu** zaškrtnutá možnost; necháte, protože se jedná.</span><span class="sxs-lookup"><span data-stu-id="e8d84-118">Notice that hello **Alternate Email Address** option is checked; leave it as it is.</span></span>
   
    ![Samoobslužné resetování hesla](./media/active-directory-b2c-reference-sspr/sspr.png)
6. <span data-ttu-id="e8d84-120">Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="e8d84-120">Click **Save** at hello bottom of hello page.</span></span> <span data-ttu-id="e8d84-121">Hotovo!</span><span class="sxs-lookup"><span data-stu-id="e8d84-121">You're done!</span></span>

<span data-ttu-id="e8d84-122">tootest, použít funkci "Spustit nyní" hello na všechny zásady přihlášení, který má místní účty jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="e8d84-122">tootest, use hello "Run now" feature on any sign-in policy that has local accounts as an identity provider.</span></span> <span data-ttu-id="e8d84-123">Na přihlášení místní účet hello stránka (kde zadáte e-mailovou adresu a heslo, nebo uživatelské jméno a heslo), klikněte na tlačítko **nelze získat přístup k účtu?** tooverify hello prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8d84-123">On hello local account sign-in page (where you enter an email address and password, or a username and password), click **Can't access your account?** tooverify hello consumer experience.</span></span>

> [!NOTE]
> <span data-ttu-id="e8d84-124">Hello stránky resetování hesla pomocí samoobslužné služby lze přizpůsobit pomocí hello [firemního brandingu funkce](../active-directory/active-directory-add-company-branding.md).</span><span class="sxs-lookup"><span data-stu-id="e8d84-124">hello self-service password reset pages can be customized by using hello [company branding feature](../active-directory/active-directory-add-company-branding.md).</span></span>
> 
> 

