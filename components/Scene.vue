<template>
  
  <canvas id="bg"></canvas>

</template>

<script>
import * as THREE from 'three'
import { FBXLoader } from 'three-stdlib'

export default {

  
  data(){
    return {

      renderer: null,
      scene: new THREE.Scene(),
      camera: new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 ),
      controls: null,
      clock: new THREE.Clock(),

    

      scrollPos: 0,
      scrollPercent: 0,

      // LIGHTS
      userLight: null,
      dollLight: null,
      hemiLight: null,

      // GEO

      doll:null,

      ground: null,

      startCube: null,
      lookHere: null,

      // ANIMATION
      mixer: null,
    }
  },

  mounted(){
    this.init()
    this.animate()
  },

  methods: {


    init() {
      
      this.renderer = new THREE.WebGL1Renderer({canvas: document.querySelector('#bg')})
      this.renderer.setPixelRatio(window.devicePixelRatio)
      this.renderer.setSize(window.innerWidth, window.innerHeight)
      this.renderer.shadowMap.enabled = true
      this.renderer.shadowMap.type = THREE.PCFSoftShadowMap

      this.camera.position.set( 0, 30, 0 )
      // const helper = new THREE.CameraHelper( this.camera )
      // this.scene.add(helper )


      // this.scene.background = new THREE.Color('skyblue')
      // this.scene.fog = new THREE.FogExp2( 'white', 0.0005 );


      // this.userLight = new THREE.PointLight( 0xffffff, 1, 300, 2);
      // this.userLight.position.set( 100, 50, 0 )
      // this.scene.add( this.userLight )
      this.dollLight = new THREE.PointLight('blue', 1, 1000, 1)
      this.dollLight.castShadow = true
      this.scene.add( this.dollLight )

      this.dollLight.shadow.mapSize.width = 512; // default
      this.dollLight.shadow.mapSize.height = 512; // default
      this.dollLight.shadow.camera.near = 0.5; // default
      this.dollLight.shadow.camera.far = 500; // default

      // this.hemiLight = new THREE.HemisphereLight( 0xffffff, 0xffffff, 0.6 )
      // // this.hemiLight.color.setHSL( 0.6, 1, 0.6 )
      // this.hemiLight.groundColor.setHSL( 0.095, 1, 0.75 )
      // this.hemiLight.position.set( 0, 50, 0 )
      // this.scene.add( this.hemiLight )



      // GEO
      
      // const geometry = new THREE.SphereGeometry( 20, 64, 32, 6, 6.3)
      // const boxPic = new THREE.BoxGeometry( 50, 50, 50)
      // const myPic = new THREE.TextureLoader().load('mypic.jpg')
      // this.startCube = new THREE.Mesh( boxPic, new THREE.MeshStandardMaterial( { roughness: .6, map: myPic } ))
      // this.startCube.position.z = this.camera.position.z -200
      // this.startCube.position.y = this.camera.position.y +20
      // this.startCube.castShadow = true
      // this.scene.add( this.startCube )

      const groundGeo = new THREE.PlaneGeometry(10000, 10000)
      const groundMat = new THREE.MeshStandardMaterial( { color: 'gray',  } );
      this.ground = new THREE.Mesh( groundGeo, groundMat );
      this.ground.position.y = -9;
      this.ground.rotation.x = - Math.PI / 2;
      this.ground.receiveShadow = true;
      this.scene.add( this.ground );

      // LOOK HERE
      this.lookHere = new THREE.Object3D()
      this.lookHere.position.y = 50
      this.lookHere.position.z = this.camera.position.z - 100
      this.lookHere.position.x = this.camera.position.x 
      this.scene.add( this.lookHere )

      
      // BALLSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS
      // Array(300).fill().forEach(this.addBall)

      const objNoteb = new FBXLoader();

        objNoteb.load('kneeling_pointing.fbx', (el) => {
          
          this.doll = el

          this.mixer = new THREE.AnimationMixer( this.doll );

					const action = this.mixer.clipAction( this.doll.animations[ 0 ] );
          console.log(action)
					action.play();
          
          this.doll.traverse((child)=>{
            if (child instanceof THREE.Mesh) {
              console.log(child)
              // apply texture
              child.material = new THREE.MeshStandardMaterial( { color: 'white', wireframe: true } )
              child.material.needsUpdate = true;
              child.castShadow = true
              child.receiveShadow = false
            }
          })
          // this.doll.castShadow = true
          // this.doll.receiveShadow = false
          this.doll.position.y = -10
          this.doll.position.z = this.camera.position.z - 150 
          this.doll.position.x = this.camera.position.x -100
          this.scene.add(this.doll)
          this.scrolling()
        })

      document.body.appendChild( this.renderer.domElement )

      // this.controls = new FirstPersonControls( this.camera, this.renderer.domElement )
      // this.controls.movementSpeed = 100
      // this.controls.lookSpeed = 0.2

      window.addEventListener( 'resize', this.onWindowResize )

      document.body.onscroll = this.scrolling

      window.scrollTo({ top: 0, behavior: 'smooth' })
    },

    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize( window.innerWidth, window.innerHeight );
    },



    scrolling(){

      this.scrollPercent = ((document.documentElement.scrollTop || document.body.scrollTop) / ((document.documentElement.scrollHeight || document.body.scrollHeight) - document.documentElement.clientHeight)) * 100
      // console.log(this.scrollPercent)

      if (this.scrollPercent===0) {
        this.camera.position.set( 0, 30, 0 )
        this.lookHere.position.y = 50
        this.lookHere.position.z = this.camera.position.z - 100
        this.lookHere.position.x = this.camera.position.x 
      }

      if (this.scrollPercent<60) {
        try{this.mixer.setTime( this.scrollPercent*0.03 )}catch{}
      }

      // console.log(this.scrollPos)
      this.camera.position.z -= (this.animHandler(1,20))
      this.camera.position.y += (this.animHandler(1,20))
      this.lookHere.position.y+= (this.animHandler(1, 20))
      this.lookHere.position.x-= (this.animHandler(1, 20))

      this.lookHere.position.x-= (this.animHandler(20, 40))*4
      this.lookHere.position.z+= (this.animHandler(40, 60))*4
      
      // this.camera.position.y = -this.scrollPos +100
      this.dollLight.position=this.lookHere.position

      this.scrollPos = this.scrollPercent

    },

    animHandler(s, e){
      const actual = this.scrollPercent
      const later = this.scrollPos
      if (actual>=s&&actual<=e) {
        const data = actual-later
        // console.log(actual, later, data)
        return data
      }
      else{
        return 0
      }
    },

    addBall(){
      const randomBall = new THREE.Mesh( new THREE.SphereGeometry( 10), new THREE.MeshLambertMaterial( {roughness: 0, color: 'white' }))
      const [x,y,z] = Array(3).fill().map(()=> THREE.MathUtils.randFloatSpread(5000))
      randomBall.position.set(x,y,z)
      this.scene.add(randomBall)
    },

    animate() {
      requestAnimationFrame(this.animate)
      this.render()
    },


    render() {

      this.camera.lookAt(this.lookHere.position)
      
      this.renderer.render( this.scene, this.camera )

    },


  }

}
</script>

<style scoped>

  canvas {
    width: 100vw;
    position: fixed;
    top: 0;
    left: 0;
  }

</style>