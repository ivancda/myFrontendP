<template>
  
  <canvas id="bg"></canvas>

</template>

<script>
import * as THREE from 'three'
import { FirstPersonControls, Sky, Water} from 'three-stdlib'

export default {

  
  data(){
    return {

      renderer: null,
      scene: new THREE.Scene(),
      camera: new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 ),
      controls: null,
      clock: new THREE.Clock(),

      sun: null,
      water: null,

      scrollPos: null,

      userLight: null,

      // GEO
      startCube: null,
      ballOne: null,
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

      this.camera.position.set( 100, 50, 0 )
      // const helper = new THREE.CameraHelper( this.camera )
      // this.scene.add(helper )


      // this.scene.background = new THREE.Color('skyblue')
      this.scene.fog = new THREE.FogExp2( 'white', 0.0005 );


      this.userLight = new THREE.PointLight( 0xffffff, 1, 100, 2);
      this.userLight.position.set( 100, 50, 0 )
      this.scene.add( this.userLight );




      // Water

      const waterGeometry = new THREE.PlaneGeometry( 10000, 10000 );


      this.water = new Water(
        waterGeometry,
        {
          textureWidth: 256,
          textureHeight: 256,
          waterNormals: new THREE.TextureLoader().load( 'waternormals.jpg', function ( texture ) {

            texture.wrapS = texture.wrapT = THREE.RepeatWrapping;

          } ),
          sunDirection: new THREE.Vector3(),
          sunColor: 0xffffff,
          waterColor: 0x001e0f,
          distortionScale: 3.7,
          // fog: this.scene.fog !== undefined
        }
      );
      
      this.water.rotation.x = - Math.PI / 2;

      // this.scene.add( this.water );


      // Skybox
      this.sun = new THREE.Vector3()

      const sky = new Sky();
      sky.scale.setScalar( 100000 );
      // this.scene.add( sky );

      const skyUniforms = sky.material.uniforms;

      skyUniforms.turbidity.value = 10;
      skyUniforms.rayleigh.value = 1;
      skyUniforms.mieCoefficient.value = 0.005;
      skyUniforms.mieDirectionalG.value = 0.2;

      const parameters = {
        elevation: 2,
        azimuth: 180
      };

      const pmremGenerator = new THREE.PMREMGenerator( this.renderer );

      const phi = THREE.MathUtils.degToRad( 90   - parameters.elevation );
      const theta = THREE.MathUtils.degToRad( parameters.azimuth );

      this.sun.setFromSphericalCoords( 1, phi, theta );

      sky.material.uniforms.sunPosition.value.copy( this.sun );
      this.water.material.uniforms.sunDirection.value.copy( this.sun ).normalize();

      this.scene.environment = pmremGenerator.fromScene( sky ).texture;






      // GEO
      // const geometry = new THREE.SphereGeometry( 20, 64, 32, 6, 6.3)
      const boxPic = new THREE.BoxGeometry( 50, 50, 50)
      const myPic = new THREE.TextureLoader().load('mypic.jpg')
      this.startCube = new THREE.Mesh( boxPic, new THREE.MeshStandardMaterial( { roughness: .6, map: myPic } ))
      this.scene.add( this.startCube )

      // const ballGeo = new THREE.SphereGeometry( 20)
      this.ballOne = new THREE.Object3D()
      this.scene.add( this.ballOne )

      
      
      Array(200).fill().forEach(this.addBall)

      // const mtlNoteb = new MTLLoader();
      // const objNoteb = new TDSLoader();

      // mtlNoteb.load('laptop/MacBookPro.mtl', (ele) => {
        // console.log(ele)
        
        // objNoteb.setMaterials(ele)
        // objNoteb.load('butterfly.3DS', (el) => {
          

          

          // el.position.y = 100
          // el.position.z = this.camera.position.z + 100 
          // el.position.x = this.camera.position.x 
          // console.log(el)
          // this.scene.add(el)
        // })
      // })





      document.body.appendChild( this.renderer.domElement )

      this.controls = new FirstPersonControls( this.camera, this.renderer.domElement )
      this.controls.movementSpeed = 100
      this.controls.lookSpeed = 0.2

      window.addEventListener( 'resize', this.onWindowResize )

      document.body.onscroll = this.scrolling
    

      this.scrolling()
      this.startCube.position.z = this.camera.position.z - 200
      this.startCube.position.x = 180

    },

    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize( window.innerWidth, window.innerHeight );
    },



    scrolling(){
      const p = window.scrollY
      if (p !== 0) {this.scrollPos = (Math.round(p))}
      else{this.scrollPos = p}
      

      // console.log(document.body.getBoundingClientRect())

      const x = this.camera.position.x;
      const z = this.camera.position.z;
      this.camera.position.x = (x * Math.cos(.01) + z * Math.sin(.01)) 
      this.camera.position.z = (z * Math.cos(.01) - x * Math.sin(.01)) 

      this.camera.position.y = -this.scrollPos

      this.startCube.position.y = -this.scrollPos

      this.ballOne.position.y = -this.scrollPos

    },
    

    addBall(){
      const randomBall = new THREE.Mesh( new THREE.SphereGeometry( 20), new THREE.MeshStandardMaterial( {roughness: 0, color: 'white' }))
      const [x,y,z] = Array(3).fill().map(()=> THREE.MathUtils.randFloatSpread(10000))
      randomBall.position.set(x,y,z)
      this.scene.add(randomBall)
    },

    animate() {
      requestAnimationFrame(this.animate)
      this.render()
    },


    render() {

      const time = performance.now() * 0.001;

      // this.mesh.position.y = Math.sin( time ) * 20 + 5;
      this.startCube.rotation.x = time * 0.5;
      this.startCube.rotation.z = time * 0.51;

      this.userLight.position.x = this.camera.position.x
      this.userLight.position.y = this.camera.position.y
      this.userLight.position.z = this.camera.position.z

      this.water.material.uniforms.time.value += 1.0 / 60.0;

      this.controls.update( this.clock.getDelta() )

      this.camera.lookAt(this.ballOne.position)
      
      this.renderer.render( this.scene, this.camera );

    },


  }



}
</script>

<style scoped>

  canvas {
    position: fixed;
    top: 0;
    left: 0;
  }

</style>