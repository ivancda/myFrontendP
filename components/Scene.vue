<template>
    <div>
      <canvas id="bg"></canvas>

      <!-- <div class="devBox">
        <button @click="getCameraPos">camera pos</button>
      </div> -->
    </div>
</template>

<script>
import * as THREE from 'three'
import { FirstPersonControls, OBJLoader, ImprovedNoise } from 'three-stdlib'
import { gsap } from "gsap"


export default {

  props:{
    pos: {
      type: Number,
      required: false,
      default: null
    },
  },
  
  data(){
    return {

      renderer: null,
      scene: new THREE.Scene(),
      camera: new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 ),
      controls: null,
      clock: new THREE.Clock(),

      // LIGHTS
      light1: null,
      light2: null,
      light3: null,
      light4: null,

      // GEO
      ground: null,
      lookHere: null,
      worldWidth: 30, 
      worldDepth: 30,
      mesh: null,
      theta: 0,

      boxTest: null,

      // PHYSICS
      physics: null,

      // FRAMES
      frame1: null,
      frame2: null,
      frame3: null,
      frame4: null,
      frame5: null,
      frame6: null,
      frame7: null,
      frame8: null,

      // FRAMES LIGHTS

      liteF1:null,
      liteF2:null,
      liteF3:null,
      liteF4:null,
      liteF5:null,
      liteF6:null,

    }
  },

  watch:{
    pos(val){
      // console.log(val)
      this.moveCamera(val)
    },
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

      // console.log(this.camera)
      // this.camera.setLens(10)
      // this.camera.fov = 60
      this.camera.position.set(135,180,-300)

      // this.scene.fog = new THREE.FogExp2( 'black', 0.0025 )

      // LIGHTS
      // const testLight = new THREE.AmbientLight('lightgray')
      // this.scene.add(testLight)
      const sphere = new THREE.SphereGeometry(3)
      this.light1 = new THREE.PointLight( '#0D00A4', 1, 350, 2 )
      this.light1.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: '#0D00A4' } ) ) )
      this.light1.position.set( 0, 70, -40 )
      this.light1.castShadow = true
      this.scene.add( this.light1 )
      this.light1.shadow.mapSize.width = 1024
      this.light1.shadow.mapSize.height = 1024

      this.light2 = new THREE.PointLight( '#FFFFFF', 1, 150, 2 )
      this.light2.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: '#FFFFFF' } ) ) )
      this.light2.position.set( 75, 100, -75 )
      this.light2.castShadow = true
      this.scene.add( this.light2 )
      this.light2.shadow.mapSize.width = 1024
      this.light2.shadow.mapSize.height = 1024

      this.light3 = new THREE.PointLight( '#D1345B', 1, 350, 2 )
      this.light3.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: '#D1345B' } ) ) )
      this.light3.position.set( -75, 100, 75 )
      this.light3.castShadow = true
      this.scene.add( this.light3 )
      this.light3.shadow.mapSize.width = 1024
      this.light3.shadow.mapSize.height = 1024

      // this.light4 = new THREE.PointLight( 'white', 1, 350, 2 )
      // this.light4.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'red' } ) ) )
      // this.light4.position.set( -75, 100, -75 )
      // this.light4.castShadow = true
      // this.scene.add( this.light4 )
      // this.light4.shadow.mapSize.width = 1024
      // this.light4.shadow.mapSize.height = 1024

      // const light = new THREE.AmbientLight( 0x404040 ); // soft white light
      // this.scene.add( light );


      // GROUND
      const data = this.generateHeight( this.worldWidth, this.worldDepth )
      const groundGeometry = new THREE.PlaneGeometry( 1000, 1000, this.worldWidth - 1, this.worldDepth - 1 )
				groundGeometry.rotateX( - Math.PI / 2 )
				const vertices = groundGeometry.attributes.position.array
				for ( let i = 0, j = 0, l = vertices.length; i < l; i ++, j += 3 ) {
					vertices[ j + 1 ] = data[ i ] * 3
				}
      this.mesh = new THREE.Mesh( groundGeometry, new THREE.MeshStandardMaterial( {  color:'lightblue', roughness:.9  } ) )
      this.mesh.position.y-=0
      this.mesh.receiveShadow=true
      this.scene.add( this.mesh )

      // BOX
      const houseGeo = new THREE.BoxGeometry(300,150,300)
      const houseText = new THREE.MeshStandardMaterial({ color:'white' })
      houseText.roughness = 1
      const house = new THREE.Mesh( houseGeo, houseText)
      house.position.y=50
      house.material.side = THREE.DoubleSide
      // this.scene.add(house)

      // LOOK HERE
      this.lookHere = new THREE.Object3D()
      this.lookHere.position.y = 130
      this.scene.add( this.lookHere )

      // RABBIT MODEL
      const rabLoader = new OBJLoader()
      rabLoader.load('Toelo_Real.obj',(el)=>{
        const rabbit = el
        rabbit.traverse((child)=>{
          if (child instanceof THREE.Mesh) {
            // console.log(child)
            // apply texture
            child.material = new THREE.MeshPhysicalMaterial( { color: 'white', roughness:.1, clearcoat: .8, metalness: 0 } )
            child.material.needsUpdate = true
            child.castShadow = true
            child.receiveShadow = false
          }
        })
        const rabLight = new THREE.PointLight('red', 0, 200, 1)
        rabLight.position.set(0, 60, -60)
        rabbit.position.y = 1
        this.scene.add(rabbit, rabLight)
      })

      this.addFrames()

      // BALLSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS
      Array(200).fill().forEach(this.addBall)

      this.camera.lookAt(this.lookHere.position)

      this.controls = new FirstPersonControls(this.camera, this.renderer.domElement)
      this.controls.movementSpeed = 150;
      this.controls.lookSpeed = 0.1;

      document.body.appendChild( this.renderer.domElement )
      window.addEventListener( 'resize', this.onWindowResize )
    },

      // FRAMES
    addFrames(){
      const frameGeo = new THREE.PlaneGeometry(50,50)
      const loader = new THREE.TextureLoader()
      // const frameGeo = new THREE.SphereGeometry(10)
      // const frameText = new THREE.MeshStandardMaterial({color:'#FFFFFF'})
      // const frameLight = new THREE.PointLight('green', 1, 150, 1)
      
      this.frame1 = new THREE.Mesh(frameGeo, new THREE.MeshStandardMaterial({map:loader.load('imgs/1tracker.png')}))
      this.liteF1 = new THREE.PointLight('white', .1, 100, 1)
      this.liteF1.castShadow = true
      this.liteF1.position.set(155, 40, 90)
      this.frame1.position.set(140,40,100)
      this.frame1.rotateY(  Math.PI/3*2  )
      this.frame1.material.side = THREE.DoubleSide
      this.frame1.castShadow = true
      this.scene.add(this.frame1, this.liteF1)

      this.frame2 = new THREE.Mesh(frameGeo, new THREE.MeshStandardMaterial({map:loader.load('imgs/2loop.png')}))
      this.liteF2 = new THREE.PointLight('white', .1, 100, 1)
      this.liteF2.castShadow = true
      this.liteF2.position.set(0,40,-200)
      this.frame2.position.set(-10,40,-190)
      this.frame2.rotateY(  Math.PI/4*3 )
      this.frame2.material.side = THREE.DoubleSide
      this.frame2.castShadow = true
      this.scene.add(this.frame2, this.liteF2)

      this.frame3 = new THREE.Mesh(frameGeo, new THREE.MeshStandardMaterial({map:loader.load('imgs/3this.png')}))
      this.liteF3 = new THREE.PointLight('white', .1, 100, 1)
      this.liteF3.castShadow = true
      this.liteF3.position.set(145,40,-110)
      this.frame3.position.set(130,40,-100)
      this.frame3.rotateY(  Math.PI/3*2  )
      this.frame3.material.side = THREE.DoubleSide
      this.frame3.castShadow = true
      this.scene.add(this.frame3,this.liteF3)

      this.frame4 = new THREE.Mesh(frameGeo, new THREE.MeshStandardMaterial({map:loader.load('imgs/4rest.png')}))
      this.liteF4 = new THREE.PointLight('white', .1, 100, 1)
      this.liteF4.castShadow = true
      this.liteF4.position.set(-50,40,170)
      this.frame4.position.set(-30,40,180)
      this.frame4.rotateY(  -Math.PI/3*2  )
      this.frame4.material.side = THREE.DoubleSide
      this.frame4.castShadow=true
      this.scene.add(this.frame4, this.liteF4)

      this.frame5 = new THREE.Mesh(frameGeo, new THREE.MeshStandardMaterial({map:loader.load('imgs/5todo.png')}))
      this.liteF5 = new THREE.PointLight('white', .1, 100, 1)
      this.liteF5.castShadow = true
      this.liteF5.position.set(-195,40,-80)
      this.frame5.position.set(-180,40,-90)
      this.frame5.material.side = THREE.DoubleSide
      this.frame5.rotateY(  -Math.PI /3 )
      this.frame5.castShadow = true
      this.scene.add(this.frame5, this.liteF5)

      this.frame6 = new THREE.Mesh(frameGeo, new THREE.MeshStandardMaterial({map:loader.load('imgs/6mysql.jpeg')}))
      this.liteF6 = new THREE.PointLight('white', .1, 100, 1)
      this.liteF6.castShadow = true
      this.liteF6.position.set(-110,40,40)
      this.frame6.position.set(-90,40,40)
      this.frame6.rotateY(-Math.PI/1.7)
      this.frame6.material.side = THREE.DoubleSide
      this.frame6.castShadow = true
      this.scene.add(this.frame6, this.liteF6)

      // this.frame7 = new THREE.Mesh(frameGeo, frameText)
      // this.frame7.position.set(-149,40,-33)
      // this.frame7.rotateY(  Math.PI /2 )
      // this.scene.add(this.frame7)

      // this.frame8 = new THREE.Mesh(frameGeo, frameText)
      // this.frame8.position.set(-149,40,-100)
      // this.frame8.rotateY(  Math.PI /2 )
      // this.scene.add(this.frame8)
    },

    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize( window.innerWidth, window.innerHeight );
    },

    animate() {
      requestAnimationFrame(this.animate)
      this.render()
    },

    render() {
      this.camera.lookAt(this.lookHere.position)

      this.animGeo()

      // this.theta += 0.1;
      // this.camera.position.x = 100 * Math.sin( THREE.MathUtils.degToRad( this.theta ) )
      // this.camera.position.y = (10 * Math.sin( THREE.MathUtils.degToRad( this.theta ) ))+50
      // this.camera.position.z = 100 * Math.cos( THREE.MathUtils.degToRad( this.theta ) )

      // this.camera.position.z = Math.cos( Date.now() * 0.0003 ) * 60 
      // this.camera.position.x = Math.sin( Date.now() * 0.0003 ) * 60
      // this.camera.position.y = (Math.sin( Date.now() * 0.0005 ) * 10)+50
      

      // this.controls.update( this.clock.getDelta() )

      this.renderer.render( this.scene, this.camera )

    },

    animGeo(){
      const time = Date.now() * 0.0008

      // this.boxTest.position.y = 40;
      // this.boxTest.rotation.x = time * 0.5;
      // this.boxTest.rotation.z = time * 0.51;

      this.light1.position.z = Math.cos( time * 0.44 ) * 200 +10
      this.light1.position.x = Math.sin( time * 1 ) * 200 -10

      this.light2.position.z = Math.cos( time * 0.76 ) * 50 
      this.light2.position.x = -Math.sin( time * 1 ) * 30 +10

      this.light3.position.z = Math.sin( time * .19378 ) * 150 -10
      this.light3.position.x = Math.cos( time * .19432 ) * 150
      
      this.frame1.position.y = Math.sin(time*3) +40
      this.frame2.position.y = -Math.sin(time*3) +40
      this.frame3.position.y = Math.sin(time*3) +40
      this.frame4.position.y = -Math.sin(time*3) +40
      this.frame5.position.y = Math.sin(time*3) +40
      this.frame6.position.y = -Math.sin(time*3) +40

    },

    addBall(){
      const randomBall = new THREE.Mesh( new THREE.SphereGeometry( 3), new THREE.MeshBasicMaterial( { color: 'white' }))
      const [x,y,z] = Array(3).fill().map(()=> THREE.MathUtils.randFloatSpread(3000))
      randomBall.position.set(x,y+1600,z)
      this.scene.add(randomBall)
    },

    generateHeight( width, height ) {
      let seed = Math.PI / 4;
      window.Math.random = function () {
        const x = Math.sin( seed ++ ) * 10000;
        return x - Math.floor( x );
      };
      const size = width * height
      const data = new Uint8Array( size );
      const perlin = new ImprovedNoise() 
      const z = Math.random() * 10;
      let quality = 0.04;
      for ( let j = 0; j < 4; j ++ ) {
        for ( let i = 0; i < size; i ++ ) {
          const x = i % width
          const y = ~ ~ ( i / width );
          data[ i ] += Math.abs( perlin.noise( x / quality, y / quality*10, z ) * quality * 1.75 );
        }
        quality *= 5;
      }
      return data;
    },

    moveCamera(pos){
      // console.log(this.pos)
      let lookHereDestiny = {}
      let cameraDestiny = {x:135,y:180,z:-300}
      this.turnOffLites()
      switch (pos) {
        case -2:
          lookHereDestiny = {x:0,y:40,z:0}
          cameraDestiny = {x:0,y:800,z:-800}
          break;
        case -1:
          lookHereDestiny = {x:0,y:20,z:0}
          cameraDestiny = {x:-80,y:40,z:-40}
          break;
        case 1:
          lookHereDestiny = this.frame1.position
          cameraDestiny = {x:235,y:40,z:80}
          gsap.to(this.liteF1, {duration:1, intensity:1})
          break;
        case 2:
          lookHereDestiny = this.frame2.position
          cameraDestiny = {x:55,y:40,z:-250}
          gsap.to(this.liteF2, {duration:1, intensity:1})
          break;
        case 3:
          lookHereDestiny = this.frame3.position
          cameraDestiny = {x:215,y:40,z:-120}
          gsap.to(this.liteF3, {duration:1, intensity:1})
          break;
        case 4:
          lookHereDestiny = this.frame4.position
          cameraDestiny = {x:-105,y:40,z:135}
          gsap.to(this.liteF4, {duration:1, intensity:1})
          break;
        case 5:
          lookHereDestiny = this.frame5.position
          cameraDestiny = {x:-265,y:40,z:-70}
          gsap.to(this.liteF5, {duration:1, intensity:1})
          break;
        case 6:
          lookHereDestiny = this.frame6.position
          cameraDestiny = {x:-175,y:40,z:5}
          gsap.to(this.liteF6, {duration:1, intensity:1})
          break;
        case 7:
          lookHereDestiny = this.frame7.position
          break;
        case 8:
          lookHereDestiny = this.frame8.position
          break;

        default:
          lookHereDestiny = {x:0,y:130,z:0}
          cameraDestiny = {x:135,y:180,z:-300}
          break;
      }

      gsap.to(this.lookHere.position, {duration:1,x:lookHereDestiny.x})
      gsap.to(this.lookHere.position, {duration:1,y:lookHereDestiny.y})
      gsap.to(this.lookHere.position, {duration:1,z:lookHereDestiny.z})

      gsap.to(this.camera.position, {duration:1, x:cameraDestiny.x})
      gsap.to(this.camera.position, {duration:1, y:cameraDestiny.y})
      gsap.to(this.camera.position, {duration:1, z:cameraDestiny.z})


      
    },

    turnOffLites(){
      gsap.to(this.liteF1, {duration:1, intensity:0})
      gsap.to(this.liteF2, {duration:1, intensity:0})
      gsap.to(this.liteF3, {duration:1, intensity:0})
      gsap.to(this.liteF4, {duration:1, intensity:0})
      gsap.to(this.liteF5, {duration:1, intensity:0})
      gsap.to(this.liteF6, {duration:1, intensity:0})

    },

    getCameraPos(){
      const x = this.camera.position.x
      const y = this.camera.position.y
      const z = this.camera.position.z
      console.log({x,y,z})
      const textVec = new THREE.Vector3()
      this.camera.getWorldDirection(textVec)
      console.log(this.camera);
      console.log(textVec)
    }

  }

}
</script>

<style scoped>

  canvas {
    width: 99vw;
    position: fixed;
    top: 0;
    left: 0;
    /* z-index: 2; */
  }

  /* .devBox{
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
  } */

</style>