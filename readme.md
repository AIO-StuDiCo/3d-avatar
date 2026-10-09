# Barry Animation

- Maak Timeline Animations in blender met keyframes (I)
- Export: GLTF, alle Actions als één action (andere optie is activeerbare actions per animatie, bv. open deksel, open midden)
- De pagina toont `barry-meshy-large.glb` met model controls en AR.

## TODO

- hard edges aangeven / no smoothing in blender
- animatie hoofdje/armpjes
- aan komen rijden???
- simplify vertices even further?

## Mixamo

- for characters with t-pose, mixamo needs OBJ, FBX or ZIP
- PBR textures will crash, you can load the pure model without textures
- export OBJ from blender with correct height (1.7m), basepoint at feet. FBX keeps crashing.

Mixamo workflow:

- Download the first animation “With Skin” (this contains your model plus the rig).
- Download every other animation “Without Skin”. This gives you only the animation data, and the files are smaller.
- Blender: Import the FBX files. Start with the model with skin. Set scale to 100 to prevent tiny models.
- Re-attach the texture: click file>append. Append the .blend file that HAS the correct texture. Only append the MATERIAL folder.
    - Then select the mesh (not the armature), click the material tab, add a material, choose the loaded material.
- Importing the other animations: import FBX (choose the animation fbx that has no skin, set scale to 100).
- ?? Each animation appears as an action. Give actions logical names. Use the NLA Editor (Nonlinear Animation) to push the actions into strips and arrange them in sequence.
- Then export the whole thing to GLB for modelviewer (gltf)
- Unity: Import all the FBX files and set up an Animator or state machine to switch between the animations.

## Modelviewer animations

If there are more animations you can switch with javascript

```js
viewer.addEventListener('load', () => console.log(viewer.availableAnimations));
```
```js
<model-viewer id="viewer" src="character.glb" autoplay></model-viewer>

<button onclick="setAnim('Idle')">Idle</button>
<button onclick="setAnim('Walk')">Walk</button>

<script>
  const viewer = document.getElementById('viewer');

  function setAnim(name) {
    viewer.animationName = name;
    viewer.play();
  }
</script>
```

- Crossfade between animations: `add animation-crossfade-duration="300"` (milliseconds) 
- Loop control: `viewer.play({ repetitions: 1 })` plays once. Default is infinite.
- Pause/resume: `viewer.pause()` and `viewer.play()`.