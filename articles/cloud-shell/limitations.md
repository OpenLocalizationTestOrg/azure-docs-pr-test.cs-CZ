---
title: "omezení aaaAzure prostředí cloudu (Preview) | Microsoft Docs"
description: "Přehled omezení prostředí cloudu Azure"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 8462b0b9850fcde790a386433009439bbab52c0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-cloud-shell"></a><span data-ttu-id="450ac-103">Omezení prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="450ac-103">Limitations of Azure Cloud Shell</span></span>
<span data-ttu-id="450ac-104">Azure Cloud prostředí má následující hello známé omezení:</span><span class="sxs-lookup"><span data-stu-id="450ac-104">Azure Cloud Shell has hello following known limitations:</span></span>

## <a name="system-state-and-persistence"></a><span data-ttu-id="450ac-105">Stav systému a trvalost</span><span class="sxs-lookup"><span data-stu-id="450ac-105">System state and persistence</span></span>
<span data-ttu-id="450ac-106">Hello počítač, který obsahuje vaše cloudové prostředí relace je dočasný a bude recyklována po vaše relace je neaktivní po dobu 20 minut.</span><span class="sxs-lookup"><span data-stu-id="450ac-106">hello machine that provides your Cloud Shell session is temporary, and it is recycled after your session is inactive for 20 minutes.</span></span> <span data-ttu-id="450ac-107">Cloudové prostředí vyžaduje toobe sdílenou složku soubor připojené.</span><span class="sxs-lookup"><span data-stu-id="450ac-107">Cloud Shell requires a file share toobe mounted.</span></span> <span data-ttu-id="450ac-108">Vaše předplatné v důsledku toho musí být schopný tooset až tooaccess prostředky úložiště cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="450ac-108">As a result, your subscription must be able tooset up storage resources tooaccess Cloud Shell.</span></span> <span data-ttu-id="450ac-109">Mezi další aspekty patří:</span><span class="sxs-lookup"><span data-stu-id="450ac-109">Other considerations include:</span></span>
* <span data-ttu-id="450ac-110">S použitím připojené úložiště, pouze změny v rámci vaší `$Home` adresáře nebo `clouddrive` adresáře jsou nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="450ac-110">With mounted storage, only modifications within your `$Home` directory or `clouddrive` directory are persisted.</span></span>
* <span data-ttu-id="450ac-111">Sdílené složky může být připojen pouze z uvnitř vaší [přiřazené oblast](persisting-shell-storage.md#mount-a-new-clouddrive).</span><span class="sxs-lookup"><span data-stu-id="450ac-111">File shares can be mounted only from within your [assigned region](persisting-shell-storage.md#mount-a-new-clouddrive).</span></span>
* <span data-ttu-id="450ac-112">Soubory Azure podporuje pouze místně redundantního úložiště a účty geograficky redundantní úložiště.</span><span class="sxs-lookup"><span data-stu-id="450ac-112">Azure Files supports only locally redundant storage and geo-redundant storage accounts.</span></span>

## <a name="user-permissions"></a><span data-ttu-id="450ac-113">Uživatelská oprávnění</span><span class="sxs-lookup"><span data-stu-id="450ac-113">User permissions</span></span>
<span data-ttu-id="450ac-114">Máte nastavená oprávnění jako běžní uživatelé bez přístupu sudo.</span><span class="sxs-lookup"><span data-stu-id="450ac-114">Permissions are set as regular users without sudo access.</span></span> <span data-ttu-id="450ac-115">Všechny instalace mimo vaší `$Home` directory neuchovává.</span><span class="sxs-lookup"><span data-stu-id="450ac-115">Any installation outside your `$Home` directory will not persist.</span></span>
<span data-ttu-id="450ac-116">I když některé příkazy v rámci hello `clouddrive` adresář, jako třeba `git clone`, nemáte správná oprávnění, vaše `$Home` directory nemá oprávnění.</span><span class="sxs-lookup"><span data-stu-id="450ac-116">Although certain commands within hello `clouddrive` directory, such as `git clone`, do not have proper permissions, your `$Home` directory does have permissions.</span></span>

## <a name="browser-support"></a><span data-ttu-id="450ac-117">Podpora prohlížeče</span><span class="sxs-lookup"><span data-stu-id="450ac-117">Browser support</span></span>
<span data-ttu-id="450ac-118">Cloudové prostředí podporuje hello nejnovější verze Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox a Apple Safari.</span><span class="sxs-lookup"><span data-stu-id="450ac-118">Cloud Shell supports hello latest versions of Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox, and Apple Safari.</span></span> <span data-ttu-id="450ac-119">Safari v privátním režimu není podporována.</span><span class="sxs-lookup"><span data-stu-id="450ac-119">Safari in private mode is not supported.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="450ac-120">Kopírování a vkládání</span><span class="sxs-lookup"><span data-stu-id="450ac-120">Copy and paste</span></span>
<span data-ttu-id="450ac-121">CTRL + C a Ctrl + V nefungují jako zkopírujte a vložte zkratky v prostředí cloudu na počítače s Windows, použijte kombinaci kláves Ctrl + Insert a Shift + Insert toocopy a vložení v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="450ac-121">Ctrl+C and Ctrl+V do not function as copy/paste shortcuts in Cloud Shell on Windows machines, use Ctrl+Insert and Shift+Insert toocopy and paste respectively.</span></span>

<span data-ttu-id="450ac-122">Klikněte pravým tlačítkem kopírování a vkládání možnosti jsou dostupné i, ale funkce klikněte pravým tlačítkem na je subjektu toobrowser specifické schránky přístup.</span><span class="sxs-lookup"><span data-stu-id="450ac-122">Right-click copy-and-paste options are also available, but right-click function is subject toobrowser-specific clipboard access.</span></span>

## <a name="editing-bashrc"></a><span data-ttu-id="450ac-123">Úpravy .bashrc</span><span class="sxs-lookup"><span data-stu-id="450ac-123">Editing .bashrc</span></span>
<span data-ttu-id="450ac-124">Proveďte opatrní při úpravě .bashrc, tak může způsobit neočekávané chyby v prostředí cloudu.</span><span class="sxs-lookup"><span data-stu-id="450ac-124">Take caution when editing .bashrc, doing so can cause unexpected errors in Cloud Shell.</span></span>

## <a name="bashhistory"></a><span data-ttu-id="450ac-125">.bash_history</span><span class="sxs-lookup"><span data-stu-id="450ac-125">.bash_history</span></span>
<span data-ttu-id="450ac-126">Historii bash příkazy může být nekonzistentní z důvodu narušení relace prostředí cloudu nebo souběžných relací.</span><span class="sxs-lookup"><span data-stu-id="450ac-126">Your history of bash commands may be inconsistent because of Cloud Shell session disruption or concurrent sessions.</span></span>

## <a name="usage-limits"></a><span data-ttu-id="450ac-127">Omezení využití</span><span class="sxs-lookup"><span data-stu-id="450ac-127">Usage limits</span></span>
<span data-ttu-id="450ac-128">Cloudové prostředí je určen pro případy použití interaktivní.</span><span class="sxs-lookup"><span data-stu-id="450ac-128">Cloud Shell is intended for interactive use cases.</span></span> <span data-ttu-id="450ac-129">V důsledku toho ukončení relací neinteraktivní všechny dlouhodobé bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="450ac-129">As a result, any long-running non-interactive sessions are ended without warning.</span></span>

## <a name="network-connectivity"></a><span data-ttu-id="450ac-130">Připojení k síti</span><span class="sxs-lookup"><span data-stu-id="450ac-130">Network connectivity</span></span>
<span data-ttu-id="450ac-131">Všechny latence v prostředí cloudu je připojení k Internetu toolocal předmětu, pokračuje v prostředí cloudu tooattempt toocarry se žádné pokyny odeslána.</span><span class="sxs-lookup"><span data-stu-id="450ac-131">Any latency in Cloud Shell is subject toolocal internet connectivity, Cloud Shell continues tooattempt toocarry out any instructions sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="450ac-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="450ac-132">Next steps</span></span>
[<span data-ttu-id="450ac-133">Rychlý start cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="450ac-133">Cloud Shell Quickstart</span></span>](quickstart.md)
