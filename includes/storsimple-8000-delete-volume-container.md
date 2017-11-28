<!--author=alkohli last changed: 01/13/17-->

<span data-ttu-id="c9d44-101">Pokud kontejner svazků má přidružené svazky, offline režimu tyto svazky nejdřív.</span><span class="sxs-lookup"><span data-stu-id="c9d44-101">If the volume container has associated volumes, take those volumes offline first.</span></span> <span data-ttu-id="c9d44-102">Postupujte podle kroků v [do offline režimu svazku](../articles/storsimple/storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="c9d44-102">Follow the steps in [Take a volume offline](../articles/storsimple/storsimple-manage-volumes.md#take-a-volume-offline).</span></span> <span data-ttu-id="c9d44-103">Jakmile jsou tyto svazky v režimu offline, můžete je odstranit.</span><span class="sxs-lookup"><span data-stu-id="c9d44-103">After the volumes are offline, you can delete them.</span></span> <span data-ttu-id="c9d44-104">Po kontejner svazků má žádné přidružené svazky, odstraňte kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="c9d44-104">When the volume container has no associated volumes, delete the volume container.</span></span> <span data-ttu-id="c9d44-105">Proveďte následující postup se odstranit kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="c9d44-105">Perform the following procedure to delete a volume container.</span></span>

#### <a name="to-delete-a-volume-container"></a><span data-ttu-id="c9d44-106">Chcete-li odstranit kontejner svazků</span><span class="sxs-lookup"><span data-stu-id="c9d44-106">To delete a volume container</span></span>
1. <span data-ttu-id="c9d44-107">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c9d44-107">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="c9d44-108">Vyberte a klikněte na zařízení a potom přejděte na **Nastavení > Správa > kontejnery svazků**.</span><span class="sxs-lookup"><span data-stu-id="c9d44-108">Select and click the device and then go to **Settings > Manage > Volume containers**.</span></span>

    ![Okno kontejnery svazku](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. <span data-ttu-id="c9d44-110">Ze seznamu tabulkové kontejnery svazků vyberte kontejner svazků, které chcete odstranit, klikněte pravým tlačítkem na **...**  a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c9d44-110">From the tabular list of volume containers, select the volume container you want to delete, right click **...** and then select **Delete**.</span></span>

    ![Odstranit kontejneru svazků](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. <span data-ttu-id="c9d44-112">Pokud kontejner svazků má žádné přidružené svazky, pak mohl být odstraněn.</span><span class="sxs-lookup"><span data-stu-id="c9d44-112">If a volume container has no associated volumes, then it can be deleted.</span></span> <span data-ttu-id="c9d44-113">Po zobrazení výzvy k potvrzení, zkontrolujte a zaškrtněte políčko s informacemi o tom důsledcích odstranění kontejneru svazků.</span><span class="sxs-lookup"><span data-stu-id="c9d44-113">When prompted for confirmation, review and select the checkbox stating the impact of deleting the volume container.</span></span> <span data-ttu-id="c9d44-114">Klikněte na tlačítko **odstranit** poté odstranit kontejner svazků.</span><span class="sxs-lookup"><span data-stu-id="c9d44-114">Click **Delete** to then delete the volume container.</span></span>

    ![Potvrzení odstranění](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

<span data-ttu-id="c9d44-116">Seznam kontejnery svazků je aktualizovat tak, aby odrážela kontejneru odstraněné svazku.</span><span class="sxs-lookup"><span data-stu-id="c9d44-116">The list of volume containers is updated to reflect the deleted volume container.</span></span>

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


