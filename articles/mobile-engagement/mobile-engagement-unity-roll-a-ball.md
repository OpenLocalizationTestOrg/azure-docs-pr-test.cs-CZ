---
title: "Vrácení Unity míč kurzu"
description: "Postup vytvoření klasického Unity vrátit míč hra, který je nezbytný předpoklad pro všechny kurzy Mobile Engagement Unity"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0afd0eca-f74a-43aa-bba8-436a0265c312
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 6392d1f780b1bc2348fee5947550b05e86ea4de2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="75806-103"><a id="unity-roll-a-ball"></a>Vytvoření Unity vrátit míč hry</span><span class="sxs-lookup"><span data-stu-id="75806-103"><a id="unity-roll-a-ball"></a>Create Unity Roll a Ball game</span></span>
<span data-ttu-id="75806-104">Tento kurz vás provede hlavní kroky pro mírně upravenou [Unity vrátit míč kurzu](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span><span class="sxs-lookup"><span data-stu-id="75806-104">This tutorial walks through the main steps for a slightly modified [Unity Roll a Ball tutorial](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span></span> <span data-ttu-id="75806-105">Tato ukázka hra se skládá z kulovým 'player, objekt, který řídí uživatele aplikace a hry cílem je, shromažďování' collectible objekty podle kolize objektu player se tyto collectible objekty.</span><span class="sxs-lookup"><span data-stu-id="75806-105">This sample game consists of a spherical 'player' object which is controlled by the app user and the objective of the game is to 'collect' collectible objects by colliding the player object with these collectible objects.</span></span> <span data-ttu-id="75806-106">Toto předpokládá základní znalost prostředí editor Unity.</span><span class="sxs-lookup"><span data-stu-id="75806-106">This assumes basic familiarity with Unity editor environment.</span></span> <span data-ttu-id="75806-107">Pokud narazíte na problémy s pak naleznete v úplné kurzu.</span><span class="sxs-lookup"><span data-stu-id="75806-107">If you run into any issues then you should refer to the full tutorial.</span></span> 

### <a name="setting-up-the-game"></a><span data-ttu-id="75806-108">Nastavení hra</span><span class="sxs-lookup"><span data-stu-id="75806-108">Setting up the game</span></span>
<span data-ttu-id="75806-109">Následující kroky jsou určeny z [kurzu Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="75806-109">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span></span>

1. <span data-ttu-id="75806-110">Otevřete **Unity Editor** a klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="75806-110">Open **Unity Editor** and click **New**.</span></span> 
   
    ![][51] 
2. <span data-ttu-id="75806-111">Zadejte **název projektu** & **umístění**, vyberte **3D** a klikněte na tlačítko **vytvořit projekt**.</span><span class="sxs-lookup"><span data-stu-id="75806-111">Provide a **Project name** & **Location**, select **3D** and click **Create project**.</span></span>
   
    ![][52]
3. <span data-ttu-id="75806-112">Uložit scény výchozí právě vytvořené jako součást nového projektu jako s názvem **MiniGame** v rámci nového  **\_scény** ve složce **prostředky** složky:</span><span class="sxs-lookup"><span data-stu-id="75806-112">Save the default scene just created as part of the new project as with the name **MiniGame** within a new **\_Scenes** folder under **Assets** folder:</span></span>
   
    ![][53]
4. <span data-ttu-id="75806-113">Vytvoření **3D objektu -> roviny** jako přehrávání pole a přejmenujte tento objekt roviny jako **základů**</span><span class="sxs-lookup"><span data-stu-id="75806-113">Create a **3D Object -> Plane** as the playing field and rename this plane object as **Ground**</span></span>
   
    ![][1]
5. <span data-ttu-id="75806-114">Resetovat pro tuto komponentu transformace **základů** objektu tak, aby se v původu.</span><span class="sxs-lookup"><span data-stu-id="75806-114">Reset the transform component for this **Ground** object so that it is at the Origin.</span></span> 
   
    ![][3]
6. <span data-ttu-id="75806-115">Zrušte zaškrtnutí políčka **zobrazit mřížky** z **si nabídky** pro **základů** objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-115">Uncheck **Show Grid** from **Gizmos menu** for the **Ground** object.</span></span>
   
    ![][4]
7. <span data-ttu-id="75806-116">Aktualizace **škálování** pro součásti **základů** objekt, který má být [X = 2, Y = 1, Z = 2].</span><span class="sxs-lookup"><span data-stu-id="75806-116">Update the **Scale** component for the **Ground** object to be [X = 2,Y = 1, Z = 2].</span></span> 
   
    ![][5]
8. <span data-ttu-id="75806-117">Přidejte nový **3D objektu -> oblasti** projektu a přejmenování této oblasti objekt jako **Player**.</span><span class="sxs-lookup"><span data-stu-id="75806-117">Add a new **3D Object -> Sphere** to the project and rename this sphere object as **Player**.</span></span> 
   
    ![][6]
9. <span data-ttu-id="75806-118">Vyberte **Player** objekt a klikněte na tlačítko **resetovat transformace** podobné roviny objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-118">Select the **Player** object and click **Reset Transform** similar to the Plane object.</span></span> 
10. <span data-ttu-id="75806-119">Aktualizace **transformace -> Pozice -> souřadnici Y** součásti pro Player Y as 0,5.</span><span class="sxs-lookup"><span data-stu-id="75806-119">Update **Transform -> Position -> Y Coordinate** component for the Player Y as 0.5.</span></span>  
    
    ![][7]
11. <span data-ttu-id="75806-120">Vytvořit novou složku s názvem **materiály** v projektu, kde vytvoříme materiálu pro barevný přehrávač.</span><span class="sxs-lookup"><span data-stu-id="75806-120">Create a new folder called **Materials** in the project where we will create the material to color the player.</span></span> 
12. <span data-ttu-id="75806-121">Vytvořte novou **materiálu** názvem **pozadí** v této složce.</span><span class="sxs-lookup"><span data-stu-id="75806-121">Create a new **Material** called **Background** in this folder.</span></span> 
    
    ![][8]
13. <span data-ttu-id="75806-122">Aktualizovat barvu materiálu aktualizací **Albedo** vlastnost ho.</span><span class="sxs-lookup"><span data-stu-id="75806-122">Update the color of the material by updating the **Albedo** property of it.</span></span> <span data-ttu-id="75806-123">Můžete vybrat hodnoty RGB [0,32,64].</span><span class="sxs-lookup"><span data-stu-id="75806-123">You can select the RGB values of [0,32,64].</span></span> 
    
    ![][9]
14. <span data-ttu-id="75806-124">Přetáhněte tomto materiálu do zobrazení scény použít barvu, která **základů** objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-124">Drag this material into the scene view to apply color to the **Ground** object.</span></span> 
    
    ![][10]
15. <span data-ttu-id="75806-125">Nakonec aktualizujte **transformace -> otočení -> Y** na 60 na objekt směrovou Light pro přehlednost.</span><span class="sxs-lookup"><span data-stu-id="75806-125">Finally update the **Transform -> Rotation -> Y** to 60 on the Directional Light object for clarity.</span></span> 
    
    ![][12]

### <a name="moving-the-player"></a><span data-ttu-id="75806-126">Přesunutí player</span><span class="sxs-lookup"><span data-stu-id="75806-126">Moving the player</span></span>
<span data-ttu-id="75806-127">Následující kroky jsou určeny z [kurzu Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="75806-127">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span></span>

1. <span data-ttu-id="75806-128">Přidat **RigidBody** součásti na **Player** objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-128">Add a **RigidBody** component to the **Player** object.</span></span> 
   
    ![][13]
2. <span data-ttu-id="75806-129">Vytvořit novou složku s názvem **skripty** v projektu.</span><span class="sxs-lookup"><span data-stu-id="75806-129">Create a new folder called **Scripts** in the Project.</span></span> 
3. <span data-ttu-id="75806-130">Klikněte na tlačítko **přidat součást -> nový skript -> C# skript**.</span><span class="sxs-lookup"><span data-stu-id="75806-130">Click **Add Component-> New Script -> C# Script**.</span></span> <span data-ttu-id="75806-131">Pojmenujte ji **PlayerController**a klikněte na tlačítko **vytvořit a přidat**.</span><span class="sxs-lookup"><span data-stu-id="75806-131">Name it **PlayerController**, and click **Create and Add**.</span></span> <span data-ttu-id="75806-132">Tato akce vytvoří a připojí k objektu Player skript.</span><span class="sxs-lookup"><span data-stu-id="75806-132">This will create and attach a script to the Player object.</span></span>  
   
    ![][14]
4. <span data-ttu-id="75806-133">Tento skript v části přesunout **skripty** složky v projektu.</span><span class="sxs-lookup"><span data-stu-id="75806-133">Move this script under the **Scripts** folder in the project.</span></span> 
5. <span data-ttu-id="75806-134">Otevřete skript pro úpravy v editoru vaše oblíbené skriptu, aktualizujte kód skriptu následujícím kódem a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="75806-134">Open the script for editing in your favorite script editor, update the script code with the following code and save it.</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
6. <span data-ttu-id="75806-135">Všimněte si, že používá skript výše **rychlost** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="75806-135">Note that the script above uses a **Speed** property.</span></span> <span data-ttu-id="75806-136">V editoru Unity aktualizujte vlastnost rychlostí 10.</span><span class="sxs-lookup"><span data-stu-id="75806-136">In the Unity editor, update the speed property to 10.</span></span>  
   
    ![][15]
7. <span data-ttu-id="75806-137">Stiskněte tlačítko **přehrání** v editoru Unity.</span><span class="sxs-lookup"><span data-stu-id="75806-137">Hit **Play** in the Unity Editor.</span></span> <span data-ttu-id="75806-138">Teď by měla být schopné řídit míč pomocí klávesnice a měl by otočit a pohyb.</span><span class="sxs-lookup"><span data-stu-id="75806-138">Now you should be able to control the ball using the keyboard and it should rotate and move around.</span></span> 

### <a name="moving-the-camera"></a><span data-ttu-id="75806-139">Přesunutí kamera</span><span class="sxs-lookup"><span data-stu-id="75806-139">Moving the camera</span></span>
<span data-ttu-id="75806-140">Následující kroky jsou určeny z [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) a bude tie **hlavní fotoaparát** k **Player** objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-140">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) and will tie the **Main Camera** to the **Player** object.</span></span> 

1. <span data-ttu-id="75806-141">Aktualizace **Transform.Position** být X = 0, Y = 10.5, Z =-10.</span><span class="sxs-lookup"><span data-stu-id="75806-141">Update the **Transform.Position** to be X = 0,  Y = 10.5, Z=-10.</span></span>  
2. <span data-ttu-id="75806-142">Aktualizace **Transform.Rotation** být X = 45, Y = 0, Z = 0.</span><span class="sxs-lookup"><span data-stu-id="75806-142">Update the **Transform.Rotation** to be X = 45, Y = 0, Z = 0.</span></span>  
   
    ![][16]
3. <span data-ttu-id="75806-143">Přidat nový skript volá **CameraController** k **MainCamera** a přesunout ji pod složky skriptů.</span><span class="sxs-lookup"><span data-stu-id="75806-143">Add a new script called **CameraController** to the **MainCamera** and move it under the Scripts folder.</span></span> 
   
    ![][17]
4. <span data-ttu-id="75806-144">Otevřete pro úpravy skriptu a do něj přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="75806-144">Open up the script for editing and add the following code in it:</span></span>
   
        using UnityEngine;
        using System.Collections;
   
        public class CameraController : MonoBehaviour {
   
            public GameObject player;
   
            private Vector3 offset;
   
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
   
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
5. <span data-ttu-id="75806-145">V prostředí Unity - přetáhněte proměnnou Player do přihrádky Player pro objekt fotoaparát hlavní tak, aby dva jsou přidruženy jednu na druhou.</span><span class="sxs-lookup"><span data-stu-id="75806-145">In Unity environment - drag the Player variable into the Player slot for the Main Camera object so that the two are associated with one another.</span></span> 
   
    ![][18]
6. <span data-ttu-id="75806-146">Teď Pokud začít přehrávat v editoru Unity a otočení objektu Player míč pak uvidíte fotoaparát následující v pohyb.</span><span class="sxs-lookup"><span data-stu-id="75806-146">Now if you hit Play in the Unity editor and rotate the Player Ball object then you will see the Camera following it in the movement.</span></span>  

### <a name="setting-up-the-play-area"></a><span data-ttu-id="75806-147">Nastavení oblasti Play</span><span class="sxs-lookup"><span data-stu-id="75806-147">Setting up the Play area</span></span>
<span data-ttu-id="75806-148">Následující kroky jsou určeny z [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="75806-148">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span></span> <span data-ttu-id="75806-149">Vytvoříme bariéry, které obaluje základů tak, aby objekt Player míč není odkládací oblast play v jeho přesunu.</span><span class="sxs-lookup"><span data-stu-id="75806-149">We will create the Walls surrounding the Ground so that the Player Ball object doesn't drop off the play area in its movement.</span></span> 

1. <span data-ttu-id="75806-150">Klikněte na tlačítko **vytvořit -> vytvořit prázdnou -> objekt herní** a pojmenujte ji **bariéry**</span><span class="sxs-lookup"><span data-stu-id="75806-150">Click **Create -> Create Empty -> Game Object** and name it **Walls**</span></span>
   
    ![][19]
2. <span data-ttu-id="75806-151">V rámci tohoto objektu bariéry - vytvořit nový **3D objektu -> datové krychle** a pojmenujte ji "Wall – západ".</span><span class="sxs-lookup"><span data-stu-id="75806-151">Under this Walls object - create a new **3D Object -> Cube** and name it "West wall".</span></span> 
   
    ![][20]
3. <span data-ttu-id="75806-152">Aktualizace **transformace -> Pozice** a **transformace -> škálování** pro tento objekt Wall – západ.</span><span class="sxs-lookup"><span data-stu-id="75806-152">Update the **Transform -> Position** and **Transform -> Scale** for this West Wall object.</span></span> 
   
    ![][21]
4. <span data-ttu-id="75806-153">Duplicitní wall – západ vytvořit **wall – východ** s aktualizovaný transformace pozice a škálování.</span><span class="sxs-lookup"><span data-stu-id="75806-153">Duplicate the West wall to create an **East wall** with the updated transform position and scale.</span></span> 
   
    ![][22]
5. <span data-ttu-id="75806-154">Duplicitní wall – východ, chcete-li vytvořit **wall – sever** s aktualizovaný transformace pozice & škálování.</span><span class="sxs-lookup"><span data-stu-id="75806-154">Duplicate the East wall to create a **North wall** with the updated transform position & scale.</span></span> 
   
    ![][23]
6. <span data-ttu-id="75806-155">Duplicitní wall – sever a vytvořte **wall – jih** s aktualizovaný transformace pozice & škálování.</span><span class="sxs-lookup"><span data-stu-id="75806-155">Duplicate the North wall and create a **South wall** with the updated transform position & scale.</span></span> 
   
    ![][24]

### <a name="creating-collectible-objects"></a><span data-ttu-id="75806-156">Vytváření Collectible objektů</span><span class="sxs-lookup"><span data-stu-id="75806-156">Creating Collectible objects</span></span>
<span data-ttu-id="75806-157">Následující kroky jsou určeny z [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="75806-157">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span></span> <span data-ttu-id="75806-158">Vytvoříme některé atraktivní vypadající objekty, které tvoří sada collectible objekty, které se musí objekt Player míč ke shromažďování podle kolize s nimi.</span><span class="sxs-lookup"><span data-stu-id="75806-158">We will create some attractive looking objects which will form the set of collectible objects which the Player Ball object needs to 'collect' by colliding with them.</span></span> 

1. <span data-ttu-id="75806-159">Vytvořte novou **3D objektu datové krychle** a pojmenujte ji vyzvednutí.</span><span class="sxs-lookup"><span data-stu-id="75806-159">Create a new **3D Cube object** and name it Pickup.</span></span> 
2. <span data-ttu-id="75806-160">Upravit **transformace -> otočení** & **transformace -> škálování** výstupního objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-160">Adjust the **Transform -> Rotation** & **Transform -> Scale** of the Pickup object.</span></span> 
   
    ![][25]
3. <span data-ttu-id="75806-161">Vytvořte a připojte **nový C# skript** názvem **Rotator** do výstupního objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-161">Create and attach a **new C# Script** called **Rotator** to the Pickup object.</span></span> <span data-ttu-id="75806-162">Ujistěte se, že se skript vložit ve složce skripty.</span><span class="sxs-lookup"><span data-stu-id="75806-162">Make sure to put the script under the Scripts folder.</span></span> 
   
    ![][26]
4. <span data-ttu-id="75806-163">Otevřete tento skript pro úpravy a aktualizovat ji jako následující:</span><span class="sxs-lookup"><span data-stu-id="75806-163">Open this script for editing and update it to be the following:</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. <span data-ttu-id="75806-164">Nyní průchodu režimu Play v editoru Unity a zobrazit vaše výstupního objektu otáčení na ose.</span><span class="sxs-lookup"><span data-stu-id="75806-164">Now hit the Play mode in the Unity Editor and your Pickup object show be rotating on its axis.</span></span>
6. <span data-ttu-id="75806-165">Vytvořit novou složku s názvem **Prefabs**</span><span class="sxs-lookup"><span data-stu-id="75806-165">Create a new folder called **Prefabs**</span></span> 
   
    ![][27]
7. <span data-ttu-id="75806-166">Přetáhněte **vyzvednutí** objektu a umístí jej do složky Prefabs.</span><span class="sxs-lookup"><span data-stu-id="75806-166">Drag the **Pickup** object and put it in the Prefabs folder.</span></span>
   
    ![][28]
8. <span data-ttu-id="75806-167">Vytvořte novou **herní prázdný objekt** názvem **Pickups**.</span><span class="sxs-lookup"><span data-stu-id="75806-167">Create a new **Empty Game object** called **Pickups**.</span></span> <span data-ttu-id="75806-168">Resetovat své pozici počátek a poté přetáhněte výstupního objektu v rámci této herní objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-168">Reset its position to origin and then drag the Pickup object under this game object.</span></span>  
   
    ![][29]
9. <span data-ttu-id="75806-169">Duplicitní **vyzvednutí** objektu a šířit na **základů** objektu kolem **Player** objekt aktualizací **X & ZnaTransform.Position** hodnoty správně.</span><span class="sxs-lookup"><span data-stu-id="75806-169">Duplicate the **Pickup** object and spread it on the **Ground** object around the **Player** object by updating the **Transform.Position's X & Z** values appropriately.</span></span> 
   
    ![][30]
10. <span data-ttu-id="75806-170">Vytvoření **nové materiály** názvem **vyzvednutí** a aktualizovat ji jako červená barva aktualizací **Albedo vlastnost** podobná co jsme to udělali pro aktualizaci základů objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-170">Create a **new material** called **Pickup** and update it to be Red in color by updating the **Albedo property** similar to what we did for updating the Ground object.</span></span> 
    
    ![][31]
11. <span data-ttu-id="75806-171">Platí pro všechny objekty 4 výstupní materiál.</span><span class="sxs-lookup"><span data-stu-id="75806-171">Apply the material to all the 4 pickup objects.</span></span>
    
    ![][32]

### <a name="collecting-the-pickup-objects"></a><span data-ttu-id="75806-172">Shromažďování výstupní objekty</span><span class="sxs-lookup"><span data-stu-id="75806-172">Collecting the Pickup objects</span></span>
<span data-ttu-id="75806-173">Následující kroky jsou určeny z [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="75806-173">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span></span> <span data-ttu-id="75806-174">Přehrávač budeme aktualizovat tak, aby bylo možné na "shromažďování' výstupní objekty podle kolize s nimi.</span><span class="sxs-lookup"><span data-stu-id="75806-174">We will update the Player so that it is able to 'collect' the pickup objects by colliding with them.</span></span> 

1. <span data-ttu-id="75806-175">Otevřete **PlayerController** skript připojen k objektu Player pro úpravy a aktualizace pro následující:</span><span class="sxs-lookup"><span data-stu-id="75806-175">Open up the **PlayerController** script attached to the Player object for editing and update it to the following:</span></span>  
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour {
   
            public float speed;
   
            private Rigidbody rb;
   
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
   
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
   
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
   
                rb.AddForce (movement * speed);
            }
   
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }
2. <span data-ttu-id="75806-176">Vytvořte novou **značka** názvem **vyberte si** (musí se shodovat co je ve skriptu)</span><span class="sxs-lookup"><span data-stu-id="75806-176">Create a new **Tag** called **Pick Up** (it must match what is in the script)</span></span>  
   
    ![][33]
   
    ![][34]
3. <span data-ttu-id="75806-177">Použít **značky** k objektu Prefab vyzvednutí.</span><span class="sxs-lookup"><span data-stu-id="75806-177">Apply this **Tag** to the Prefab Pickup object.</span></span> 
   
    ![][35]
4. <span data-ttu-id="75806-178">Povolit **IsTrigger** zaškrtávací políčko pro objekt Prefab.</span><span class="sxs-lookup"><span data-stu-id="75806-178">Enable **IsTrigger** checkbox for the Prefab object.</span></span>
   
    ![][36]
5. <span data-ttu-id="75806-179">Přidáte pevné text do vyzvednutí Prefab objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-179">Add a Rigid body to Pickup Prefab object.</span></span> <span data-ttu-id="75806-180">Pro optimalizaci výkonu aktualizujeme dynamické collider statické collider, který jsme použili.</span><span class="sxs-lookup"><span data-stu-id="75806-180">For performance optimization we will update the static collider that we used to a Dynamic collider.</span></span> 
   
    ![][37]
6. <span data-ttu-id="75806-181">Nakonec zkontrolujte **IsKinematic** vlastnost prefab objektu.</span><span class="sxs-lookup"><span data-stu-id="75806-181">Finally check the **IsKinematic** property for the prefab object.</span></span> 
   
    ![][38]
7. <span data-ttu-id="75806-182">Stiskněte tlačítko **přehrání** v Unity editoru a bude možné přehrát toto **vrátit míč** herní přesunutím objektu Player pro vstup směr pomocí kláves na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="75806-182">Hit **Play** in the Unity editor and you will be able to play this **Roll a Ball** game by moving the Player object using your keyboard keys for direction input.</span></span> 

### <a name="updating-the-game-for-mobile-play"></a><span data-ttu-id="75806-183">Aktualizace hry pro mobilní play</span><span class="sxs-lookup"><span data-stu-id="75806-183">Updating the game for mobile play</span></span>
<span data-ttu-id="75806-184">Výše uvedené oddíly se dospělo k závěru základní kurzem z Unity.</span><span class="sxs-lookup"><span data-stu-id="75806-184">The sections above concluded the basic tutorial from Unity.</span></span> <span data-ttu-id="75806-185">Nyní jsme upraví hra, aby bylo mobilní zařízení popisný.</span><span class="sxs-lookup"><span data-stu-id="75806-185">Now we will modify the game to make it mobile device friendly.</span></span> <span data-ttu-id="75806-186">Upozorňujeme, že jsme vstup z klávesnice pro hra, pokud pro testování.</span><span class="sxs-lookup"><span data-stu-id="75806-186">Note that we used keyboard input for the game so far for testing.</span></span> <span data-ttu-id="75806-187">Nyní jsme upravovat ho tak, aby přehrávač jsme můžete řídit pomocí pohybu telefonu, tj.</span><span class="sxs-lookup"><span data-stu-id="75806-187">Now we will modify it so that we can control the player by using the motion of the phone i.e.</span></span> <span data-ttu-id="75806-188">pomocí zrychlení jako vstup.</span><span class="sxs-lookup"><span data-stu-id="75806-188">using Accelerometer as the input.</span></span> 

<span data-ttu-id="75806-189">Otevřete **PlayerController** skript pro úpravy a aktualizovat **FixedUpdate** metodu použít pohybu z zrychlení k přesunutí objektu Player.</span><span class="sxs-lookup"><span data-stu-id="75806-189">Open up the **PlayerController** script for editing and update the **FixedUpdate** method to use the motion from the accelerometer to move the Player object.</span></span> 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

<span data-ttu-id="75806-190">V tomto kurzu se ukončí základní herní vytvoření s Unity a můžete nasadit na zařízení podle svého výběru ve hře.</span><span class="sxs-lookup"><span data-stu-id="75806-190">This tutorial concludes a basic game creation with Unity and you can deploy this on a device of your choice to play the game.</span></span> 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png    
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png    
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png













