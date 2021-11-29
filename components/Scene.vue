<template>
  
  <div>
    <canvas id="bg"></canvas>
    <b-btn class="btn" @click="moveCamera">elBUTOON</b-btn>
  </div>
</template>

<script>
import * as THREE from 'three'
import { FirstPersonControls, ImprovedNoise } from 'three-stdlib'
import * as TWEEN from "@tweenjs/tween.js";


export default {

  
  data(){
    return {

      renderer: null,
      scene: new THREE.Scene(),
      camera: new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 ),
      controls: null,
      clock: new THREE.Clock(),


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

      texture: null,
      worldWidth: 40, 
      worldDepth: 40,
      mesh: null,
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


      this.scene.background = new THREE.Color('skyblue')
      // this.scene.fog = new THREE.FogExp2( 'skyblue', 0.0005 );


      this.userLight = new THREE.PointLight( 0xffffff, 1, 0, 1);
      this.userLight.position.set( 100, 50, 0 )
      this.scene.add( this.userLight )
      this.dollLight = new THREE.PointLight('white', 1, 0, 2)
      this.dollLight.castShadow = true
      this.scene.add( this.dollLight )

      this.dollLight.shadow.mapSize.width = 512; // default
      this.dollLight.shadow.mapSize.height = 512; // default
      this.dollLight.shadow.camera.near = 0.5; // default
      this.dollLight.shadow.camera.far = 500; // default


      // GEO

      // const geometry = new THREE.SphereGeometry( 20, 64, 32, 6, 6.3)
      // const boxPic = new THREE.BoxGeometry( 50, 50, 50)
      // const myPic = new THREE.TextureLoader().load('mypic.jpg')
      // this.startCube = new THREE.Mesh( boxPic, new THREE.MeshStandardMaterial( { roughness: .6, map: myPic } ))
      // this.startCube.position.z = this.camera.position.z -200
      // this.startCube.position.y = this.camera.position.y +20
      // this.startCube.castShadow = true
      // this.scene.add( this.startCube )
      
      const data = this.generateHeight( this.worldWidth, this.worldDepth );
      const geometry = new THREE.PlaneGeometry( 1000, 1000, this.worldWidth - 1, this.worldDepth - 1 );
				geometry.rotateX( - Math.PI / 2 );

				const vertices = geometry.attributes.position.array;

				for ( let i = 0, j = 0, l = vertices.length; i < l; i ++, j += 3 ) {

					vertices[ j + 1 ] = data[ i ] * 3;

				}

				this.texture = new THREE.CanvasTexture( this.generateTexture( data, this.worldWidth, this.worldDepth ) );
				this.texture.wrapS = THREE.ClampToEdgeWrapping;
				this.texture.wrapT = THREE.ClampToEdgeWrapping;
        
        // const groundMat = new THREE.MeshStandardMaterial( { color: 'gray',  } );
				this.mesh = new THREE.Mesh( geometry, new THREE.MeshLambertMaterial( {  color: 'gray', } ) );
        this.mesh.position.y-=200
				this.scene.add( this.mesh );













      // LOOK HERE
      this.lookHere = new THREE.Object3D()
      this.lookHere.position.y = 50
      this.lookHere.position.z = this.camera.position.z - 100
      this.lookHere.position.x = this.camera.position.x 
      this.scene.add( this.lookHere )

      
















      // BALLSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS
      Array(400).fill().forEach(this.addBall)

        // const dollD = new FBXLoader();

        //   dollD.load('doll.fbx', (el) => {
            
        //     this.doll = el

        //     this.mixer = new THREE.AnimationMixer( this.doll );

        // 		const action = this.mixer.clipAction( this.doll.animations[ 0 ] );
        // 		action.play();
            
        //     this.doll.traverse((child)=>{
        //       if (child instanceof THREE.Mesh) {
        //         console.log(child)
        //         // apply texture
        //         child.material = new THREE.MeshStandardMaterial( { color: 'black', wireframe: true } )
        //         child.material.needsUpdate = true;
        //         child.castShadow = true
        //         child.receiveShadow = false
        //       }
        //     })
        //     // this.doll.castShadow = true
        //     // this.doll.receiveShadow = false
        //     this.doll.position.y = -10
        //     this.doll.position.z = this.camera.position.z - 150 
        //     this.doll.position.x = this.camera.position.x -100
        //     this.scene.add(this.doll)
        //     this.mixer.clipAction( this.doll.animations[ 0 ] ).setDuration( 1 ).play()
        //   })

      // document.body.appendChild( this.renderer.domElement )

      this.controls = new FirstPersonControls( this.camera, this.renderer.domElement )
      this.controls.movementSpeed = 100
      this.controls.lookSpeed = 0.2

      window.addEventListener( 'resize', this.onWindowResize )

      document.body.onscroll = this.scrolling

      window.scrollTo({ top: 0, behavior: 'smooth' })
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

				let quality = 1;

				for ( let j = 0; j < 4; j ++ ) {

					for ( let i = 0; i < size; i ++ ) {

						const x = i % width
            const y = ~ ~ ( i / width );
						data[ i ] += Math.abs( perlin.noise( x / quality, y / quality, z ) * quality * 1.75 );

					}

					quality *= 5;

				}

				return data;

			},




      generateTexture( data, width, height ) {

				let context, image, imageData, shade;

				const vector3 = new THREE.Vector3( 0, 0, 0 );

				const sun = new THREE.Vector3( 1, 1, 1 );
				sun.normalize();

				const canvas = document.createElement( 'canvas' );
				canvas.width = width;
				canvas.height = height;

				context = canvas.getContext( '2d' );
				context.fillStyle = '#000';
				context.fillRect( 0, 0, width, height );

				image = context.getImageData( 0, 0, canvas.width, canvas.height );
				imageData = image.data;

				for ( let i = 0, j = 0, l = imageData.length; i < l; i += 4, j ++ ) {

					vector3.x = data[ j - 2 ] - data[ j + 2 ];
					vector3.y = 1;
					vector3.z = data[ j - width * 2 ] - data[ j + width * 2 ];
					vector3.normalize();

					shade = vector3.dot( sun );

					imageData[ i ] = ( 96 + shade * 128 ) * ( 0.5 + data[ j ] * 0.007 );
					imageData[ i + 1 ] = ( 32 + shade * 96 ) * ( 0.5 + data[ j ] * 0.007 );
					imageData[ i + 2 ] = ( shade * 96 ) * ( 0.5 + data[ j ] * 0.007 );

				}

				context.putImageData( image, 0, 0 );

				// Scaled 4x

				const canvasScaled = document.createElement( 'canvas' );
				canvasScaled.width = width * 4;
				canvasScaled.height = height * 4;

				context = canvasScaled.getContext( '2d' );
				context.scale( 4, 4 );
				context.drawImage( canvas, 0, 0 );

				image = context.getImageData( 0, 0, canvasScaled.width, canvasScaled.height );
				imageData = image.data;

				for ( let i = 0, l = imageData.length; i < l; i += 4 ) {

					const v = ~ ~ ( Math.random() * 5 );

					imageData[ i ] += v;
					imageData[ i + 1 ] += v;
					imageData[ i + 2 ] += v;

				}

				context.putImageData( image, 0, 0 );

				return canvasScaled;

			},















    addBall(){
      const randomBall = new THREE.Mesh( new THREE.SphereGeometry( 10), new THREE.MeshPhongMaterial( {roughness: 0, color: 'white' }))
      const [x,y,z] = Array(3).fill().map(()=> THREE.MathUtils.randFloatSpread(3000))
      randomBall.position.set(x,y,z)
      this.scene.add(randomBall)
    },

    moveCamera(){
      const coords = { x: this.camera.position.x, y: this.camera.position.y, z: this.camera.position.z }
      new TWEEN.Tween(coords)
      .to({ x: 1000, y: 1000, z: 1000 })
      .onUpdate(() =>
        this.camera.position.set(coords.x, coords.y, coords.z)
      )
      .start();
      console.log('foi')
    },

    animate() {
      requestAnimationFrame(this.animate)
      this.render()
    },


    render() {
      const clock = this.clock.getDelta()
      this.userLight.position = this.camera.position
      // this.camera.lookAt(this.lookHere.position)
      // this.mixer.update( clock/20 )
      this.controls.update( clock )
      this.renderer.render( this.scene, this.camera )
      TWEEN.update(clock);


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
  }

  /* .btn{
    position: absolute;
    top: 10vh;
    left: 5vw;
  } */

</style>