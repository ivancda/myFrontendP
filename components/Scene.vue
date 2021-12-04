<template>
    <div>
      <canvas id="bg"></canvas>
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

      // FRAMES
      frame1: null,
      frame2: null,
      frame3: null,
      frame4: null,
      frame5: null,
      frame6: null,
      frame7: null,
      frame8: null,
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
      this.camera.position.set(100,75,-100)

      // LIGHTS
      // const sphere = new THREE.SphereGeometry(3)
      this.light1 = new THREE.PointLight( 'white', 1, 350, 2 )
      // this.light1.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'red' } ) ) )
      this.light1.position.set( 75, 100, 75 )
      this.light1.castShadow = true
      this.scene.add( this.light1 )
      this.light1.shadow.mapSize.width = 1024
      this.light1.shadow.mapSize.height = 1024

      this.light2 = new THREE.PointLight( 'white', 1, 350, 2 )
      // this.light2.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'red' } ) ) )
      this.light2.position.set( 75, 100, -75 )
      this.light2.castShadow = true
      this.scene.add( this.light2 )
      this.light2.shadow.mapSize.width = 1024
      this.light2.shadow.mapSize.height = 1024

      this.light3 = new THREE.PointLight( 'white', 1, 350, 2 )
      // this.light3.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'red' } ) ) )
      this.light3.position.set( -75, 100, 75 )
      this.light3.castShadow = true
      this.scene.add( this.light3 )
      this.light3.shadow.mapSize.width = 1024
      this.light3.shadow.mapSize.height = 1024

      this.light4 = new THREE.PointLight( 'white', 1, 350, 2 )
      // this.light4.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'red' } ) ) )
      this.light4.position.set( -75, 100, -75 )
      this.light4.castShadow = true
      this.scene.add( this.light4 )
      this.light4.shadow.mapSize.width = 1024
      this.light4.shadow.mapSize.height = 1024

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
      this.mesh = new THREE.Mesh( groundGeometry, new THREE.MeshStandardMaterial( {  color:'lightblue', roughness:.3  } ) )
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
      this.lookHere.position.y = 70
      this.scene.add( this.lookHere )

      // RABBIT MODEL
      const rabLoader = new OBJLoader()
      rabLoader.load('Toelo_Real.obj',(el)=>{
        const rabbit = el
        rabbit.traverse((child)=>{
          if (child instanceof THREE.Mesh) {
            // console.log(child)
            // apply texture
            child.material = new THREE.MeshPhysicalMaterial( { color: 'white', roughness:0, clearcoat: 1, metalness: 1, emissive:'lightgreen' } )
            console.log(child.material.emissive)
            child.material.needsUpdate = true
            child.castShadow = true
            child.receiveShadow = false
          }
        })
        rabbit.position.y = 1
        this.scene.add(rabbit)
      })

      // BOX TEST
      // this.boxTest = new THREE.Mesh(new THREE.BoxGeometry( 30, 30, 30 ), new THREE.MeshStandardMaterial( { roughness: 0 } ))
      // this.boxTest.castShadow = true
      // this.scene.add(this.boxTest)

      // FRAMES
      const frameGeo = new THREE.PlaneGeometry(50,50)
      const frameText = new THREE.MeshStandardMaterial({color: 'red'})
      this.frame1 = new THREE.Mesh(frameGeo, frameText)
      this.frame1.position.set(100,40,149)
      this.frame1.rotateY( - Math.PI  )
      this.frame1.material.side = THREE.DoubleSide
      this.scene.add(this.frame1)

      this.frame2 = new THREE.Mesh(frameGeo, frameText)
      this.frame2.position.set(33,40,149)
      this.frame2.rotateY( - Math.PI  )
      this.scene.add(this.frame2)

      this.frame3 = new THREE.Mesh(frameGeo, frameText)
      this.frame3.position.set(-33,40,149)
      this.frame3.rotateY( - Math.PI  )
      this.scene.add(this.frame3)

      this.frame4 = new THREE.Mesh(frameGeo, frameText)
      this.frame4.position.set(-100,40,149)
      this.frame4.rotateY( - Math.PI  )
      this.scene.add(this.frame4)

      this.frame5 = new THREE.Mesh(frameGeo, frameText)
      this.frame5.position.set(-149,40,100)
      this.frame5.rotateY(  Math.PI /2 )
      this.scene.add(this.frame5)

      this.frame6 = new THREE.Mesh(frameGeo, frameText)
      this.frame6.position.set(-149,40,33)
      this.frame6.rotateY(  Math.PI /2 )
      this.scene.add(this.frame6)

      this.frame7 = new THREE.Mesh(frameGeo, frameText)
      this.frame7.position.set(-149,40,-33)
      this.frame7.rotateY(  Math.PI /2 )
      this.scene.add(this.frame7)

      this.frame8 = new THREE.Mesh(frameGeo, frameText)
      this.frame8.position.set(-149,40,-100)
      this.frame8.rotateY(  Math.PI /2 )
      this.scene.add(this.frame8)


      // BALLSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS
      Array(200).fill().forEach(this.addBall)


      this.camera.lookAt(this.lookHere.position)


      this.controls = new FirstPersonControls(this.camera, this.renderer.domElement)
      this.controls.movementSpeed = 150;
      this.controls.lookSpeed = 0.1;

      document.body.appendChild( this.renderer.domElement )
      window.addEventListener( 'resize', this.onWindowResize )
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
      // const time = Date.now() * 0.0008

      // this.boxTest.position.y = 40;
      // this.boxTest.rotation.x = time * 0.5;
      // this.boxTest.rotation.z = time * 0.51;

      // this.theta += 0.1;
      // this.camera.position.x = 100 * Math.sin( THREE.MathUtils.degToRad( this.theta ) )
      // this.camera.position.y = (10 * Math.sin( THREE.MathUtils.degToRad( this.theta ) ))+50
      // this.camera.position.z = 100 * Math.cos( THREE.MathUtils.degToRad( this.theta ) )

      // this.camera.position.z = Math.cos( Date.now() * 0.0003 ) * 60 
      // this.camera.position.x = Math.sin( Date.now() * 0.0003 ) * 60
      // this.camera.position.y = (Math.sin( Date.now() * 0.0005 ) * 10)+50

      // this.light1.position.z = Math.cos( time * 0.44 ) * 50 +10
      // this.light1.position.x = Math.sin( time * 1 ) * 50 -10

      // this.light2.position.z = Math.cos( time * 0.76 ) * 50 
      // this.light2.position.x = Math.sin( time * 1 ) * 30 +10

      // this.light3.position.z = Math.sin( time * 1 ) * 30 -10
      // this.light3.position.x = Math.cos( time * 0.89 ) * 30 

      // this.controls.update( this.clock.getDelta() )

      this.renderer.render( this.scene, this.camera )

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
      let destiny = {}
      switch (pos) {
        case -2:
          destiny = {x:0,y:40,z:0}
          break;
        case -1:
          destiny = {x:0,y:20,z:0}
          break;
        case 1:
          destiny = this.frame1.position
          break;
        case 2:
          destiny = this.frame2.position
          break;
        case 3:
          destiny = this.frame3.position
          break;
        case 4:
          destiny = this.frame4.position
          break;
        case 5:
          destiny = this.frame5.position
          break;
        case 6:
          destiny = this.frame6.position
          break;
        case 7:
          destiny = this.frame7.position
          break;
        case 8:
          destiny = this.frame8.position
          break;

        default:
          destiny = {x:0,y:60,z:0}
          break;
      }

      gsap.to(this.lookHere.position, {duration:1,x:destiny.x})
      gsap.to(this.lookHere.position, {duration:1,y:destiny.y})
      gsap.to(this.lookHere.position, {duration:1,z:destiny.z})

      if (pos===0) {
        gsap.to(this.camera.position, {duration:1,x:100})
        gsap.to(this.camera.position, {duration:1,y:75})
        gsap.to(this.camera.position, {duration:1,z:-100})
        console.log('pos 0')
      }
      else if(pos===-1){
        gsap.to(this.camera.position, {duration:1,x:-80})
        gsap.to(this.camera.position, {duration:1,y:40})
        gsap.to(this.camera.position, {duration:1,z:-40})
        console.log('pos 0')
      }
      else if(pos===-2){
        gsap.to(this.camera.position, {duration:1,x:-200})
        gsap.to(this.camera.position, {duration:1,y:150})
        gsap.to(this.camera.position, {duration:1,z:-400})
        console.log('pos 0')
      }
      else if(pos>0&&pos<5) {
        gsap.to(this.camera.position, {duration:1,x:destiny.x})
        gsap.to(this.camera.position, {duration:1,y:destiny.y})
        gsap.to(this.camera.position, {duration:1,z:destiny.z-60})
        console.log('wall1')
      }
      else if(pos>=5&&pos<10){
        gsap.to(this.camera.position, {duration:1,x:destiny.x+60})
        gsap.to(this.camera.position, {duration:1,y:destiny.y})
        gsap.to(this.camera.position, {duration:1,z:destiny.z})
        console.log('wall2')
      }
      else{
        // gsap.to(this.camera.position, {duration:1,x:60})
        // gsap.to(this.camera.position, {duration:1,y:75})
        // gsap.to(this.camera.position, {duration:1,z:-50})
      }
      
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
    /* z-index: -1; */
  }

  

</style>