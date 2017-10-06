---
title: "aaaUnity vrátit míč kurzu"
description: "Kroky toocreate hello classic vrátit Unity míč hra, který je nezbytný předpoklad pro všechny kurzy Mobile Engagement Unity"
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
ms.openlocfilehash: 10d923682432961207594886b08e5db60cf60d9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="beb11-103"><a id="unity-roll-a-ball"></a>Vytvoření Unity vrátit míč hry</span><span class="sxs-lookup"><span data-stu-id="beb11-103"><a id="unity-roll-a-ball"></a>Create Unity Roll a Ball game</span></span>
<span data-ttu-id="beb11-104">Tento kurz vás provede hello hlavní kroky pro mírně upravenou [Unity vrátit míč kurzu](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span><span class="sxs-lookup"><span data-stu-id="beb11-104">This tutorial walks through hello main steps for a slightly modified [Unity Roll a Ball tutorial](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span></span> <span data-ttu-id="beb11-105">Tato ukázka hra se skládá z kulovým 'player, objekt, který řídí uživatele aplikace hello a cílem hello ve hře hello je too'collect' collectible objekty podle kolize objektu player hello se tyto collectible objekty.</span><span class="sxs-lookup"><span data-stu-id="beb11-105">This sample game consists of a spherical 'player' object which is controlled by hello app user and hello objective of hello game is too'collect' collectible objects by colliding hello player object with these collectible objects.</span></span> <span data-ttu-id="beb11-106">Toto předpokládá základní znalost prostředí editor Unity.</span><span class="sxs-lookup"><span data-stu-id="beb11-106">This assumes basic familiarity with Unity editor environment.</span></span> <span data-ttu-id="beb11-107">Pokud narazíte na potíže by měl odkazovat toohello úplné kurzu.</span><span class="sxs-lookup"><span data-stu-id="beb11-107">If you run into any issues then you should refer toohello full tutorial.</span></span> 

### <a name="setting-up-hello-game"></a><span data-ttu-id="beb11-108">Nastavení hra hello</span><span class="sxs-lookup"><span data-stu-id="beb11-108">Setting up hello game</span></span>
<span data-ttu-id="beb11-109">Hello kroky jsou z hello [kurzu Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="beb11-109">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span></span>

1. <span data-ttu-id="beb11-110">Otevřete **Unity Editor** a klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="beb11-110">Open **Unity Editor** and click **New**.</span></span> 
   
    ![][51] 
2. <span data-ttu-id="beb11-111">Zadejte **název projektu** & **umístění**, vyberte **3D** a klikněte na tlačítko **vytvořit projekt**.</span><span class="sxs-lookup"><span data-stu-id="beb11-111">Provide a **Project name** & **Location**, select **3D** and click **Create project**.</span></span>
   
    ![][52]
3. <span data-ttu-id="beb11-112">Uložit scény výchozí hello právě vytvořené jako součást nového projektu hello jako s názvem hello **MiniGame** v rámci nového  **\_scény** ve složce **prostředky** složky:</span><span class="sxs-lookup"><span data-stu-id="beb11-112">Save hello default scene just created as part of hello new project as with hello name **MiniGame** within a new **\_Scenes** folder under **Assets** folder:</span></span>
   
    ![][53]
4. <span data-ttu-id="beb11-113">Vytvoření **3D objektu -> roviny** jako hello přehrávání pole a přejmenujte tento objekt roviny jako **základů**</span><span class="sxs-lookup"><span data-stu-id="beb11-113">Create a **3D Object -> Plane** as hello playing field and rename this plane object as **Ground**</span></span>
   
    ![][1]
5. <span data-ttu-id="beb11-114">Resetování hello transformace součásti pro tento **základů** objektu tak, aby se v hello původu.</span><span class="sxs-lookup"><span data-stu-id="beb11-114">Reset hello transform component for this **Ground** object so that it is at hello Origin.</span></span> 
   
    ![][3]
6. <span data-ttu-id="beb11-115">Zrušte zaškrtnutí políčka **zobrazit mřížky** z **si nabídky** pro hello **základů** objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-115">Uncheck **Show Grid** from **Gizmos menu** for hello **Ground** object.</span></span>
   
    ![][4]
7. <span data-ttu-id="beb11-116">Aktualizace hello **škálování** součásti pro hello **základů** objektu toobe [X = 2, Y = 1, Z = 2].</span><span class="sxs-lookup"><span data-stu-id="beb11-116">Update hello **Scale** component for hello **Ground** object toobe [X = 2,Y = 1, Z = 2].</span></span> 
   
    ![][5]
8. <span data-ttu-id="beb11-117">Přidejte nový **3D objektu -> oblasti** toohello projektu a přejmenování této oblasti objekt jako **Player**.</span><span class="sxs-lookup"><span data-stu-id="beb11-117">Add a new **3D Object -> Sphere** toohello project and rename this sphere object as **Player**.</span></span> 
   
    ![][6]
9. <span data-ttu-id="beb11-118">Vyberte hello **Player** objekt a klikněte na tlačítko **resetovat transformace** podobné toohello roviny objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-118">Select hello **Player** object and click **Reset Transform** similar toohello Plane object.</span></span> 
10. <span data-ttu-id="beb11-119">Aktualizace **transformace -> Pozice -> souřadnici Y** součásti pro hello Player Y jako 0,5.</span><span class="sxs-lookup"><span data-stu-id="beb11-119">Update **Transform -> Position -> Y Coordinate** component for hello Player Y as 0.5.</span></span>  
    
    ![][7]
11. <span data-ttu-id="beb11-120">Vytvořit novou složku s názvem **materiály** v projektu hello kde vytvoříme hello podstatným toocolor hello přehrávač.</span><span class="sxs-lookup"><span data-stu-id="beb11-120">Create a new folder called **Materials** in hello project where we will create hello material toocolor hello player.</span></span> 
12. <span data-ttu-id="beb11-121">Vytvořte novou **materiálu** názvem **pozadí** v této složce.</span><span class="sxs-lookup"><span data-stu-id="beb11-121">Create a new **Material** called **Background** in this folder.</span></span> 
    
    ![][8]
13. <span data-ttu-id="beb11-122">Aktualizovat barvu hello hello materiálu aktualizací hello **Albedo** vlastnost ho.</span><span class="sxs-lookup"><span data-stu-id="beb11-122">Update hello color of hello material by updating hello **Albedo** property of it.</span></span> <span data-ttu-id="beb11-123">Můžete vybrat hodnoty RGB hello [0,32,64].</span><span class="sxs-lookup"><span data-stu-id="beb11-123">You can select hello RGB values of [0,32,64].</span></span> 
    
    ![][9]
14. <span data-ttu-id="beb11-124">Přetáhněte tomto materiálu do hello scény zobrazení tooapply barva toohello **základů** objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-124">Drag this material into hello scene view tooapply color toohello **Ground** object.</span></span> 
    
    ![][10]
15. <span data-ttu-id="beb11-125">Nakonec aktualizujte hello **transformace -> otočení -> Y** too60 hello směrovou Light objektu pro přehlednost.</span><span class="sxs-lookup"><span data-stu-id="beb11-125">Finally update hello **Transform -> Rotation -> Y** too60 on hello Directional Light object for clarity.</span></span> 
    
    ![][12]

### <a name="moving-hello-player"></a><span data-ttu-id="beb11-126">Přesunutí player hello</span><span class="sxs-lookup"><span data-stu-id="beb11-126">Moving hello player</span></span>
<span data-ttu-id="beb11-127">Hello kroky jsou z hello [kurzu Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="beb11-127">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span></span>

1. <span data-ttu-id="beb11-128">Přidat **RigidBody** součást toohello **Player** objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-128">Add a **RigidBody** component toohello **Player** object.</span></span> 
   
    ![][13]
2. <span data-ttu-id="beb11-129">Vytvořit novou složku s názvem **skripty** v hello projektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-129">Create a new folder called **Scripts** in hello Project.</span></span> 
3. <span data-ttu-id="beb11-130">Klikněte na tlačítko **přidat součást -> nový skript -> C# skript**.</span><span class="sxs-lookup"><span data-stu-id="beb11-130">Click **Add Component-> New Script -> C# Script**.</span></span> <span data-ttu-id="beb11-131">Pojmenujte ji **PlayerController**a klikněte na tlačítko **vytvořit a přidat**.</span><span class="sxs-lookup"><span data-stu-id="beb11-131">Name it **PlayerController**, and click **Create and Add**.</span></span> <span data-ttu-id="beb11-132">Tato akce vytvoří a objekt skriptu toohello Player připojit.</span><span class="sxs-lookup"><span data-stu-id="beb11-132">This will create and attach a script toohello Player object.</span></span>  
   
    ![][14]
4. <span data-ttu-id="beb11-133">Přesunout tento skript v části hello **skripty** složky hello projektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-133">Move this script under hello **Scripts** folder in hello project.</span></span> 
5. <span data-ttu-id="beb11-134">Otevřete hello skript pro úpravy v editoru vaše oblíbené skriptu, aktualizujte kód skriptu hello s hello následující kód a uložte jej.</span><span class="sxs-lookup"><span data-stu-id="beb11-134">Open hello script for editing in your favorite script editor, update hello script code with hello following code and save it.</span></span> 
   
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
6. <span data-ttu-id="beb11-135">Všimněte si skriptu hello výše používá **rychlost** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="beb11-135">Note that hello script above uses a **Speed** property.</span></span> <span data-ttu-id="beb11-136">V editoru Unity hello aktualizujte too10 vlastnost rychlost hello.</span><span class="sxs-lookup"><span data-stu-id="beb11-136">In hello Unity editor, update hello speed property too10.</span></span>  
   
    ![][15]
7. <span data-ttu-id="beb11-137">Stiskněte tlačítko **přehrání** v hello Unity Editor.</span><span class="sxs-lookup"><span data-stu-id="beb11-137">Hit **Play** in hello Unity Editor.</span></span> <span data-ttu-id="beb11-138">Nyní byste měli mít toocontrol hello míč pomocí klávesnice hello a měl by otočit a pohyb.</span><span class="sxs-lookup"><span data-stu-id="beb11-138">Now you should be able toocontrol hello ball using hello keyboard and it should rotate and move around.</span></span> 

### <a name="moving-hello-camera"></a><span data-ttu-id="beb11-139">Přesunutí hello fotoaparát</span><span class="sxs-lookup"><span data-stu-id="beb11-139">Moving hello camera</span></span>
<span data-ttu-id="beb11-140">Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) a bude tie hello **hlavní fotoaparát** toohello **Player** objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-140">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) and will tie hello **Main Camera** toohello **Player** object.</span></span> 

1. <span data-ttu-id="beb11-141">Aktualizace hello **Transform.Position** toobe X = 0, Y = 10.5, Z =-10.</span><span class="sxs-lookup"><span data-stu-id="beb11-141">Update hello **Transform.Position** toobe X = 0,  Y = 10.5, Z=-10.</span></span>  
2. <span data-ttu-id="beb11-142">Aktualizace hello **Transform.Rotation** toobe X = 45, Y = 0, Z = 0.</span><span class="sxs-lookup"><span data-stu-id="beb11-142">Update hello **Transform.Rotation** toobe X = 45, Y = 0, Z = 0.</span></span>  
   
    ![][16]
3. <span data-ttu-id="beb11-143">Přidat nový skript volá **CameraController** toohello **MainCamera** a přesunout ji pod složka skripty hello.</span><span class="sxs-lookup"><span data-stu-id="beb11-143">Add a new script called **CameraController** toohello **MainCamera** and move it under hello Scripts folder.</span></span> 
   
    ![][17]
4. <span data-ttu-id="beb11-144">Otevře hello skript pro úpravy a přidejte následující kód v ní hello:</span><span class="sxs-lookup"><span data-stu-id="beb11-144">Open up hello script for editing and add hello following code in it:</span></span>
   
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
5. <span data-ttu-id="beb11-145">V prostředí Unity - přetáhněte hello Player proměnné do hello Player slotu pro objekt hlavní fotoaparát hello tak, aby hello dva jsou přidruženy jednu na druhou.</span><span class="sxs-lookup"><span data-stu-id="beb11-145">In Unity environment - drag hello Player variable into hello Player slot for hello Main Camera object so that hello two are associated with one another.</span></span> 
   
    ![][18]
6. <span data-ttu-id="beb11-146">Teď Pokud začít přehrávat v editoru hello Unity a objektu Player míč otáčení hello pak uvidíte hello následující v hello pohybů fotoaparát.</span><span class="sxs-lookup"><span data-stu-id="beb11-146">Now if you hit Play in hello Unity editor and rotate hello Player Ball object then you will see hello Camera following it in hello movement.</span></span>  

### <a name="setting-up-hello-play-area"></a><span data-ttu-id="beb11-147">Nastavení oblasti Play hello</span><span class="sxs-lookup"><span data-stu-id="beb11-147">Setting up hello Play area</span></span>
<span data-ttu-id="beb11-148">Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="beb11-148">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span></span> <span data-ttu-id="beb11-149">Vytvoříme hello bariéry, které obaluje hello základů tak, aby hello objektu Player míč není odkládací oblast play hello v jeho pohyb.</span><span class="sxs-lookup"><span data-stu-id="beb11-149">We will create hello Walls surrounding hello Ground so that hello Player Ball object doesn't drop off hello play area in its movement.</span></span> 

1. <span data-ttu-id="beb11-150">Klikněte na tlačítko **vytvořit -> vytvořit prázdnou -> objekt herní** a pojmenujte ji **bariéry**</span><span class="sxs-lookup"><span data-stu-id="beb11-150">Click **Create -> Create Empty -> Game Object** and name it **Walls**</span></span>
   
    ![][19]
2. <span data-ttu-id="beb11-151">V rámci tohoto objektu bariéry - vytvořit nový **3D objektu -> datové krychle** a pojmenujte ji "Wall – západ".</span><span class="sxs-lookup"><span data-stu-id="beb11-151">Under this Walls object - create a new **3D Object -> Cube** and name it "West wall".</span></span> 
   
    ![][20]
3. <span data-ttu-id="beb11-152">Aktualizace hello **transformace -> Pozice** a **transformace -> škálování** pro tento objekt Wall – západ.</span><span class="sxs-lookup"><span data-stu-id="beb11-152">Update hello **Transform -> Position** and **Transform -> Scale** for this West Wall object.</span></span> 
   
    ![][21]
4. <span data-ttu-id="beb11-153">Duplicitní toocreate wall – západ hello **wall – východ** s hello aktualizovat transformace pozice a škálování.</span><span class="sxs-lookup"><span data-stu-id="beb11-153">Duplicate hello West wall toocreate an **East wall** with hello updated transform position and scale.</span></span> 
   
    ![][22]
5. <span data-ttu-id="beb11-154">Duplicitní toocreate wall – východ hello **wall – sever** s hello aktualizovat transformace pozice & škálování.</span><span class="sxs-lookup"><span data-stu-id="beb11-154">Duplicate hello East wall toocreate a **North wall** with hello updated transform position & scale.</span></span> 
   
    ![][23]
6. <span data-ttu-id="beb11-155">Duplicitní wall – sever hello a vytvořte **wall – jih** s hello aktualizovat transformace pozice & škálování.</span><span class="sxs-lookup"><span data-stu-id="beb11-155">Duplicate hello North wall and create a **South wall** with hello updated transform position & scale.</span></span> 
   
    ![][24]

### <a name="creating-collectible-objects"></a><span data-ttu-id="beb11-156">Vytváření Collectible objektů</span><span class="sxs-lookup"><span data-stu-id="beb11-156">Creating Collectible objects</span></span>
<span data-ttu-id="beb11-157">Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="beb11-157">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span></span> <span data-ttu-id="beb11-158">Vytvoříme některé atraktivní vyhledávání objektů, které tvoří hello sadu collectible objektů, které objekt Player míč hello musí too'collect se podle kolize s nimi.</span><span class="sxs-lookup"><span data-stu-id="beb11-158">We will create some attractive looking objects which will form hello set of collectible objects which hello Player Ball object needs too'collect' by colliding with them.</span></span> 

1. <span data-ttu-id="beb11-159">Vytvořte novou **3D objektu datové krychle** a pojmenujte ji vyzvednutí.</span><span class="sxs-lookup"><span data-stu-id="beb11-159">Create a new **3D Cube object** and name it Pickup.</span></span> 
2. <span data-ttu-id="beb11-160">Upravit hello **transformace -> otočení** & **transformace -> škálování** hello výstupního objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-160">Adjust hello **Transform -> Rotation** & **Transform -> Scale** of hello Pickup object.</span></span> 
   
    ![][25]
3. <span data-ttu-id="beb11-161">Vytvořte a připojte **nový C# skript** názvem **Rotator** toohello výstupního objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-161">Create and attach a **new C# Script** called **Rotator** toohello Pickup object.</span></span> <span data-ttu-id="beb11-162">Ujistěte se, že skript hello tooput hello skripty ve složce.</span><span class="sxs-lookup"><span data-stu-id="beb11-162">Make sure tooput hello script under hello Scripts folder.</span></span> 
   
    ![][26]
4. <span data-ttu-id="beb11-163">Otevřete tento skript pro úpravy a aktualizovat ji toobe hello následující:</span><span class="sxs-lookup"><span data-stu-id="beb11-163">Open this script for editing and update it toobe hello following:</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. <span data-ttu-id="beb11-164">Teď se na ose otáčení přístupů hello Play režimu v hello Unity Editor a zobrazit vaše výstupního objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-164">Now hit hello Play mode in hello Unity Editor and your Pickup object show be rotating on its axis.</span></span>
6. <span data-ttu-id="beb11-165">Vytvořit novou složku s názvem **Prefabs**</span><span class="sxs-lookup"><span data-stu-id="beb11-165">Create a new folder called **Prefabs**</span></span> 
   
    ![][27]
7. <span data-ttu-id="beb11-166">Přetáhněte hello **vyzvednutí** objektu a umístí jej do složky Prefabs hello.</span><span class="sxs-lookup"><span data-stu-id="beb11-166">Drag hello **Pickup** object and put it in hello Prefabs folder.</span></span>
   
    ![][28]
8. <span data-ttu-id="beb11-167">Vytvořte novou **herní prázdný objekt** názvem **Pickups**.</span><span class="sxs-lookup"><span data-stu-id="beb11-167">Create a new **Empty Game object** called **Pickups**.</span></span> <span data-ttu-id="beb11-168">Obnovit jeho tooorigin pozice a poté přetáhněte hello výstupního objektu v rámci této herní objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-168">Reset its position tooorigin and then drag hello Pickup object under this game object.</span></span>  
   
    ![][29]
9. <span data-ttu-id="beb11-169">Duplicitní hello **vyzvednutí** objektu a šířit na hello **základů** objekt kolem hello **Player** objekt aktualizací hello **na Transform.Position X & Z**  hodnoty správně.</span><span class="sxs-lookup"><span data-stu-id="beb11-169">Duplicate hello **Pickup** object and spread it on hello **Ground** object around hello **Player** object by updating hello **Transform.Position's X & Z** values appropriately.</span></span> 
   
    ![][30]
10. <span data-ttu-id="beb11-170">Vytvoření **nové materiály** názvem **vyzvednutí** a aktualizovat ji toobe Red barevně aktualizací hello **Albedo vlastnost** podobné toowhat jsme pro aktualizaci hello základů objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-170">Create a **new material** called **Pickup** and update it toobe Red in color by updating hello **Albedo property** similar toowhat we did for updating hello Ground object.</span></span> 
    
    ![][31]
11. <span data-ttu-id="beb11-171">Použijte hello podstatným tooall hello 4 výstupní objekty.</span><span class="sxs-lookup"><span data-stu-id="beb11-171">Apply hello material tooall hello 4 pickup objects.</span></span>
    
    ![][32]

### <a name="collecting-hello-pickup-objects"></a><span data-ttu-id="beb11-172">Shromažďování hello výstupní objekty</span><span class="sxs-lookup"><span data-stu-id="beb11-172">Collecting hello Pickup objects</span></span>
<span data-ttu-id="beb11-173">Hello kroky jsou z hello [Unity kurzu](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="beb11-173">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span></span> <span data-ttu-id="beb11-174">Aktualizujeme hello Player, aby bylo možné too'collect' hello výstupní objekty podle kolize s nimi.</span><span class="sxs-lookup"><span data-stu-id="beb11-174">We will update hello Player so that it is able too'collect' hello pickup objects by colliding with them.</span></span> 

1. <span data-ttu-id="beb11-175">Otevřete hello **PlayerController** skript připojené toohello objektu Player pro úpravy a aktualizovat ji toohello následující:</span><span class="sxs-lookup"><span data-stu-id="beb11-175">Open up hello **PlayerController** script attached toohello Player object for editing and update it toohello following:</span></span>  
   
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
2. <span data-ttu-id="beb11-176">Vytvořte novou **značka** názvem **vyberte si** (musí se shodovat co je ve skriptu hello)</span><span class="sxs-lookup"><span data-stu-id="beb11-176">Create a new **Tag** called **Pick Up** (it must match what is in hello script)</span></span>  
   
    ![][33]
   
    ![][34]
3. <span data-ttu-id="beb11-177">Použít **značka** toohello Prefab vyzvednutí objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-177">Apply this **Tag** toohello Prefab Pickup object.</span></span> 
   
    ![][35]
4. <span data-ttu-id="beb11-178">Povolit **IsTrigger** zaškrtávací políčko pro objekt Prefab hello.</span><span class="sxs-lookup"><span data-stu-id="beb11-178">Enable **IsTrigger** checkbox for hello Prefab object.</span></span>
   
    ![][36]
5. <span data-ttu-id="beb11-179">Přidá objekt Prefab tooPickup pevné textu.</span><span class="sxs-lookup"><span data-stu-id="beb11-179">Add a Rigid body tooPickup Prefab object.</span></span> <span data-ttu-id="beb11-180">Pro optimalizaci výkonu bude aktualizován, použili jsme dynamické collider tooa statické collider hello.</span><span class="sxs-lookup"><span data-stu-id="beb11-180">For performance optimization we will update hello static collider that we used tooa Dynamic collider.</span></span> 
   
    ![][37]
6. <span data-ttu-id="beb11-181">Nakonec zkontrolujte hello **IsKinematic** vlastnost hello prefab objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-181">Finally check hello **IsKinematic** property for hello prefab object.</span></span> 
   
    ![][38]
7. <span data-ttu-id="beb11-182">Stiskněte tlačítko **přehrání** v editoru hello Unity a bude moct tooplay to **vrátit míč** herní přesunutím hello Player objekt, který používá vaše kláves pro směr vstup.</span><span class="sxs-lookup"><span data-stu-id="beb11-182">Hit **Play** in hello Unity editor and you will be able tooplay this **Roll a Ball** game by moving hello Player object using your keyboard keys for direction input.</span></span> 

### <a name="updating-hello-game-for-mobile-play"></a><span data-ttu-id="beb11-183">Aktualizace hello herní pro mobilní play</span><span class="sxs-lookup"><span data-stu-id="beb11-183">Updating hello game for mobile play</span></span>
<span data-ttu-id="beb11-184">Hello části výše svém hello základní kurz z Unity.</span><span class="sxs-lookup"><span data-stu-id="beb11-184">hello sections above concluded hello basic tutorial from Unity.</span></span> <span data-ttu-id="beb11-185">Nyní jsme upraví hello herní toomake ho popisný mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="beb11-185">Now we will modify hello game toomake it mobile device friendly.</span></span> <span data-ttu-id="beb11-186">Upozorňujeme, že jsme vstup z klávesnice pro hello herní dosavadní pro testování.</span><span class="sxs-lookup"><span data-stu-id="beb11-186">Note that we used keyboard input for hello game so far for testing.</span></span> <span data-ttu-id="beb11-187">Nyní jsme ji upravovat tak, aby jsme můžete řídit phone hello player pomocí hello pohybu Dobrý den, tj pomocí zrychlení jako vstup hello.</span><span class="sxs-lookup"><span data-stu-id="beb11-187">Now we will modify it so that we can control hello player by using hello motion of hello phone i.e. using Accelerometer as hello input.</span></span> 

<span data-ttu-id="beb11-188">Otevřete hello **PlayerController** skript pro úpravy a aktualizace hello **FixedUpdate** metoda toouse hello pohybu z hello zrychlení toomove hello Player objektu.</span><span class="sxs-lookup"><span data-stu-id="beb11-188">Open up hello **PlayerController** script for editing and update hello **FixedUpdate** method toouse hello motion from hello accelerometer toomove hello Player object.</span></span> 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

<span data-ttu-id="beb11-189">V tomto kurzu se ukončí základní herní vytvoření s Unity a můžete nasadit na zařízení, které vaše volba tooplay hello hra.</span><span class="sxs-lookup"><span data-stu-id="beb11-189">This tutorial concludes a basic game creation with Unity and you can deploy this on a device of your choice tooplay hello game.</span></span> 

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













