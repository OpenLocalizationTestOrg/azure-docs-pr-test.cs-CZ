---
title: aaaConfigure Docker hostitele s VirtualBox | Microsoft Docs
description: "Podrobné pokyny tooconfigure výchozí Docker instance pomocí Docker počítač a VirtualBox"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 1df2da4482444a803d05e413e019edcc57269062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Konfigurace hostitele Docker s VirtualBox
## <a name="overview"></a>Přehled
Tento článek vás provede konfiguraci výchozí instance Docker pomocí Docker počítač a VirtualBox. Pokud používáte hello [Docker pro systém Windows beta](http://beta.docker.com/), tato konfigurace není nutné.

## <a name="prerequisites"></a>Požadavky
Hello tyto nástroje musí toobe nainstalována.

* [Sada nástrojů docker](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-hello-docker-client-with-windows-powershell"></a>Konfigurace klienta Docker hello pomocí prostředí Windows PowerShell
tooconfigure Docker klienta, jednoduše otevřete prostředí Windows PowerShell a proveďte hello následující kroky:

1. Vytvořte výchozí instance docker hostitele.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Ověřte, zda text hello výchozí instance je nakonfigurovaná a spuštěná. (Byste měli vidět instanci s názvem výchozí systémem.
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![výstup ls počítač docker][0]
3. Nastavit výchozí jako aktuální hostitel hello a nakonfigurujte své prostředí.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. Zobrazte kontejnery služby active Docker hello. seznam Hello by měla být prázdná.
   
    ```PowerShell
    docker ps
    ```
   
    ![výstup ps docker][1]

> [!NOTE]
> Pokaždé, když restartujete vývojovém počítači, budete potřebovat toorestart vaše místní docker hostitele.
> toodo se problém hello následující příkaz na příkazovém řádku: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
