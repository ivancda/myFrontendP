<template>
    <canvas id="bg"></canvas>
</template>

<script>
import * as THREE from 'three'
import { ImprovedNoise, OBJLoader, CinematicCamera } from 'three-stdlib'


export default {

  
  data(){
    return {

      renderer: null,
      scene: new THREE.Scene(),
      camera: new CinematicCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 ),

      // LIGHTS
      light1: null,
      light2: null,
      light3: null,

      // GEO
      ground: null,
      lookHere: null,
      worldWidth: 30, 
      worldDepth: 30,
      mesh: null,
      theta: 0,
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

      console.log(this.camera)
      this.camera.setLens(25)
      this.camera.fov = 60

      // LIGHTS
      // const sphere = new THREE.SphereGeometry(3)
      this.light1 = new THREE.PointLight( 'red', 1, 200, 2 )
      // this.light1.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'red' } ) ) )
      this.light1.position.set( 30, 50, 20 )
      this.light1.castShadow = true
      this.scene.add( this.light1 )
      this.light1.shadow.mapSize.width = 1024
      this.light1.shadow.mapSize.height = 1024

      this.light2 = new THREE.PointLight( 'green', 1, 200, 2 )
      // this.light2.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'green' } ) ) )
      this.light2.position.set( -30, 50, 20 )
      this.light2.castShadow = true
      this.scene.add( this.light2 )
      this.light2.shadow.mapSize.width = 1024
      this.light2.shadow.mapSize.height = 1024

      this.light3 = new THREE.PointLight( 'blue', 1, 200, 2 )
      // this.light3.add( new THREE.Mesh( sphere, new THREE.MeshBasicMaterial( { color: 'blue' } ) ) )
      this.light3.position.set( 0, 50, -50 )
      this.light3.castShadow = true
      this.scene.add( this.light3 )
      this.light3.shadow.mapSize.width = 1024
      this.light3.shadow.mapSize.height = 1024

      // GROUND
      const data = this.generateHeight( this.worldWidth, this.worldDepth )
      const groundGeometry = new THREE.PlaneGeometry( 1000, 1000, this.worldWidth - 1, this.worldDepth - 1 )
				groundGeometry.rotateX( - Math.PI / 2 )
				const vertices = groundGeometry.attributes.position.array
				for ( let i = 0, j = 0, l = vertices.length; i < l; i ++, j += 3 ) {
					vertices[ j + 1 ] = data[ i ] * 3
				}
      this.mesh = new THREE.Mesh( groundGeometry, new THREE.MeshPhongMaterial( {  color:'white'  } ) )
      this.mesh.position.y-=0
      this.mesh.receiveShadow=true
      this.scene.add( this.mesh )

      // LOOK HERE
      this.lookHere = new THREE.Object3D()
      this.lookHere.position.y = 30
      this.scene.add( this.lookHere )

      // RABBIT MODEL
      const rabLoader = new OBJLoader()
      rabLoader.load('rabbit.obj',(el)=>{
        const rabbit = el
        rabbit.traverse((child)=>{
          if (child instanceof THREE.Mesh) {
            // console.log(child)
            // apply texture
            child.material = new THREE.MeshStandardMaterial( { color: 'white'} )
            child.material.needsUpdate = true;
            child.receiveShadow = true
            child.castShadow = true
            child.receiveShadow = false
          }
        })
        rabbit.position.y = 6
        this.scene.add(rabbit)
      })

      // BALLSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS
      // Array(200).fill().forEach(this.addBall)

      document.body.appendChild( this.renderer.domElement )
      window.addEventListener( 'resize', this.onWindowResize )
    },

    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize( window.innerWidth, window.innerHeight );
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

				let quality = 0.05;

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

    addBall(){
      const randomBall = new THREE.Mesh( new THREE.SphereGeometry( 3), new THREE.MeshBasicMaterial( { color: 'white' }))
      const [x,y,z] = Array(3).fill().map(()=> THREE.MathUtils.randFloatSpread(3000))
      randomBall.position.set(x,y+1500,z)
      this.scene.add(randomBall)
    },

    animate() {
      requestAnimationFrame(this.animate)
      this.render()
    },

    render() {
      this.camera.lookAt(this.lookHere.position)
      const time = Date.now() * 0.0008

      this.theta += 0.1;
      this.camera.position.x = 100 * Math.sin( THREE.MathUtils.degToRad( this.theta ) )
      this.camera.position.y = (10 * Math.sin( THREE.MathUtils.degToRad( this.theta ) ))+50
      this.camera.position.z = 100 * Math.cos( THREE.MathUtils.degToRad( this.theta ) )

      // this.camera.position.z = Math.cos( Date.now() * 0.0003 ) * 60 
      // this.camera.position.x = Math.sin( Date.now() * 0.0003 ) * 60
      // this.camera.position.y = (Math.sin( Date.now() * 0.0005 ) * 10)+50

      this.light1.position.z = Math.cos( time * 0.44 ) * 50 +10
      this.light1.position.x = Math.sin( time * 1 ) * 50 -10

      this.light2.position.z = Math.cos( time * 0.76 ) * 50 
      this.light2.position.x = Math.sin( time * 1 ) * 30 +10

      this.light3.position.z = Math.sin( time * 1 ) * 30 -10
      this.light3.position.x = Math.cos( time * 0.89 ) * 30 

      this.renderer.render( this.scene, this.camera )

    },


  }

}
</script>

<style scoped>

  canvas {
    width: 99vw;
    position: fixed;
    top: 0;
    left: 0;
    z-index: -1;
  }

</style>