<script setup lang="ts">
import { onMounted } from 'vue';
import { TresCanvas } from '@tresjs/core'
import { OrbitControls,GLTFModel } from '@tresjs/cientos'
import { BasicShadowMap, SRGBColorSpace, NoToneMapping } from 'three'

const gl = {
  clearColor: '#82DBC5',
  shadows: true,
  alpha: false,
  shadowMapType: BasicShadowMap,
  outputColorSpace: SRGBColorSpace,
  toneMapping: NoToneMapping,
}

const boxRef = shallowRef()

const { onLoop } = useRenderLoop()

onLoop(({delta}) => {
  if(boxRef.value){
    boxRef.value.value.rotation.y += delta
  }
})

//const {scene,animations}=await useGLTF("file:///home/tsilavo/T%C3%A9l%C3%A9chargements/blender-cube.glb")

onMounted(async function() {
  //const { scene, animations } = await useGLTF('https://raw.githubusercontent.com/Tresjs/assets/main/models/gltf/ugly-naked-bunny/ugly-naked-bunny-animated.gltf',)
  const { scene, animations } = await useGLTF('./heritsilavo.glb',)
  console.log(scene,animations);
 })
</script>

<template>
  <TresCanvas v-bind="gl" window-size>
    <TresPerspectiveCamera :position="[3, 2, 5]" />
    <OrbitControls />
    <Suspense>
      <GLTFModel  ref="boxRef" path="/models/heritsilavo.gltf"></GLTFModel>
    </Suspense>
    <TresDirectionalLight
      :intensity="5"
      :position="[3, 3, 3]"
    />
    <TresDirectionalLight
      :intensity="5"
      :position="[-3, 0, -3]"
    />
    <TresAmbientLight :intensity="5" />
  </TresCanvas>
</template>