# Juan de Pareja: exclusive custom skin for Marta (Hellish Quart)

A fan-made skin that works only for Marta. It runs on Marta's native 68-joint skeleton (Mafra5/Marfa), with her original moves and rapier. The look follows Velázquez's portrait of Juan de Pareja (1650), the painter who worked in his studio: a brown buttoned doublet with a peplum and shoulder wings, a wide white lace collar, dark sleeves with linen cuffs, full breeches gathered at the knee, dark stockings, bowed shoes, curly black hair and a beard.

- **Stiff doublet, soft waist.** The buttoned doublet above the waistband moves as one piece on the chest (Spine1), so the buttons and the lace collar never warp. Below it the waist blends Hips → Spine → Spine1 as on the Felipe II skin, so lunges and twists bend smoothly at the waistband and the peplum hangs naturally.
- **Breeches that follow the legs.** The full breeches were weighted mostly to the pelvis in the source and would stretch like a curtain when a leg rose. Below the hip joint each side now follows its own thigh (as on the Catalina de Erauso skin), blending back to the source weights above the knee.
- **Shoulders without gaps.** Everything past the shoulder joint belongs to the arm, so the shoulder wings hang over the sleeves when the arms drop. The doublet sides under the armpits belong to the torso (as on the Ana María de Soto skin), so the arms hang close to the body.
- **Natural neck.** The upper body is aligned at the collarbone roots, so neck, head and collar keep the source proportions.
- **Limbs and hips fitted to Marta's skeleton.** The game snaps every skin bone onto Marta's bone, so her joint positions rule. Each collarbone, upper arm (x1.17), forearm (x1.13), thigh (x0.96) and shin (x1.23) is stretched evenly along its own bone while binding, and the lower trunk is fitted so the hip joint lands on Marta's. No length difference piles up at the shoulder, elbow or hip.
- **Rigid head and hair.** Face, beard and the full curly hair form one rigid part on the head bone; the lace collar stays on the shoulders.
- **No ghost body when hit.** Marta's 17 wound overlay meshes (`DecalBloody`, `DecalSpine1`, ...) are shaped like her own body. The visibility controller keeps them hidden under the skin, together with her stock face.
- **Sharp textures within the game budget.** The 1M-triangle source is reduced to 80k triangles after welding its UV seams; each part stays under the 65k vertices the game's wound decal builder supports.
- **Fingers weighted from the geometry.** Every finger vertex follows its two nearest finger chains, blended by distance, with the phalanx given by its position along the finger, so the hand closes cleanly on the grip.

## Release

The zip contains only the four bundles (`skin_marta_juandepareja`, `pic_marta_juandepareja` and their `.manifest` files) and `README.txt`. There are no scripts. Install by copying the four files into `Hellish Quart_Data/StreamingAssets/`. `README.txt` also documents the optional manual first-person (FPP) adjustment for Marta's skin loader (4 bytes of `resources.assets`, game version of November 2026).

## Layout

- `model/JuanDePareja_Marta.blend`: the fitted result on Marta's skeleton, editable in Blender 5.3.
- `model/references/`: the four reference views.
- `model/selector_portrait.png`: the selector picture (Velázquez's portrait of Juan de Pareja, 1650, public domain).
- `model/JuanDePareja_source.glb` is not in the repository because of its size (80 MB). Place it there to rebuild from scratch.
- `blender_work/`:
  - `step1_prepare.py` welds, decimates and cleans the source, transfers its rig weights and exports the textures;
  - `step3_rig_marta.py` fits Marta's limbs and hands, rebuilds the waist, breeches, arm, shoulder and armpit weights, stiffens the doublet, stretches the limb segments and fits the hips while carrying the mesh into Marta's bind pose, and splits off the rigid head, with QA renders;
  - `verify_skeleton.py` checks the 68 bones and the weights.
- `unity_project/`: Unity 2019.4.0f1 project (menu **JuanDePareja → Construir mod**).
- `release/README.txt`: the English text shipped in the zip.

## Rebuild

Copy `Hellish Quart_Data/Managed/PlayMaker.dll` from your own installation to `unity_project/Assets/Plugins/`, then run from the repository folder (absolute paths, so Blender writes into this folder):

```powershell
$blender="C:\Program Files\Blender Foundation\Blender 5.3\blender.exe"
$root=(Get-Location).Path
$skel="$root\unity_project\Assets\JuanDeParejaMod\Editor\MartaSkeleton.json"
& $blender -b --factory-startup -P blender_work\step1_prepare.py -- "$root\model\JuanDePareja_source.glb" "$root\model\JuanDePareja_source.glb" "$root\build"
& $blender -b --factory-startup -P blender_work\step3_rig_marta.py -- "$root\build" $skel
& $blender -b --factory-startup -P blender_work\verify_skeleton.py -- "$root\build\marta\Skin.fbx" $skel
.\build.ps1
```

`build.ps1` is a developer build script and is never part of the release. It runs the Unity batch build, copies the four bundles into `release/` and creates `JuanDePareja_Marta.zip`.
