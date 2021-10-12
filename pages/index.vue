<template>

  <div class="container">

    <canvas id="bg"></canvas>

    <main>

      <header>
        <h1>Ivan Coelho</h1>
        <p>🚀 Welcome to my website!</p>
      </header>


      <blockquote>
        <p>I like making stuff and putting it on the internet</p>
      </blockquote>

      <section>
        <h2>📜 Manifesto</h2>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>

        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>

        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>

      </section>

      <section class="light">
        <h2>👩🏽‍🚀 Projects</h2>

        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>

        <h2>🏆 Accomplishments</h2>

        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>

      </section>

      <blockquote>
        <p>The best way out is always through <br>-Robert Frost</p>
      </blockquote>

      <section class="left">
        <h2>🌮 Work History</h2>

        <h3>1</h3>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
        <h3>2</h3>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
        <h3>3</h3>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>

      </section>

      <blockquote>
        <p>Thanks for watching!</p>
      </blockquote>

    </main>

  </div>
  
</template>

<script>
  import * as THREE from 'three'
  import { FirstPersonControls, Sky, Water} from 'three-stdlib'

  export default {

  data() {
    return {

      renderer: null,
      scene: new THREE.Scene(),
      camera: new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 ),
      controls: null,
      clock: new THREE.Clock(),
      plane: null,
      loader: null,
      mtlLoader: null,

      objectTest: null,

      mesh: null, 
      texture: null,
      worldWidth: 256, 
      worldDepth: 256,


      sun: null,
      water: null,

    }
  },
  mounted() {
    this.init()
    this.animate()
  },
  methods: {


    init() {
      
      this.renderer = new THREE.WebGL1Renderer({canvas: document.querySelector('#bg')})
      this.renderer.setPixelRatio(window.devicePixelRatio)
      this.renderer.setSize(window.innerWidth, window.innerHeight)

      this.camera.position.set( 100, 50, - 800 )
      // this.camera.lookAt(-50, -50)


      this.scene.background = new THREE.Color('skyblue')
      this.scene.fog = new THREE.FogExp2( 'white', 0.0005 );


      // const pointLight1 = new THREE.PointLight( 0xffffff );
      // pointLight1.position.set( 500, 500, 500 );
      // this.scene.add( pointLight1 );





      // Water

      const waterGeometry = new THREE.PlaneGeometry( 10000, 10000 );


      this.water = new Water(
        waterGeometry,
        {
          textureWidth: 512,
          textureHeight: 512,
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
      
      // this.water.position.y = 700
      this.water.rotation.x = - Math.PI / 2;

      this.scene.add( this.water );


      // Skybox
      this.sun = new THREE.Vector3()

      const sky = new Sky();
      sky.scale.setScalar( 10000 );
      this.scene.add( sky );

      const skyUniforms = sky.material.uniforms;

      console.log(skyUniforms)
      skyUniforms.turbidity.value = 10;
      skyUniforms.rayleigh.value = 2;
      skyUniforms.mieCoefficient.value = 0.005;
      skyUniforms.mieDirectionalG.value = 0.08;

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

      


      













      const geometry = new THREE.SphereGeometry( 20, 64, 32, 6, 6.3)
      

      this.mesh = new THREE.Mesh( geometry, new THREE.MeshStandardMaterial( { roughness: 0, color:'white' } ));
      this.mesh.position.y = 100
      this.mesh.position.x = 10
      this.mesh.position.z = -1000
      this.scene.add( this.mesh );


      


      





      document.body.appendChild( this.renderer.domElement )

      this.controls = new FirstPersonControls( this.camera, this.renderer.domElement )
      this.controls.movementSpeed = 100
      this.controls.lookSpeed = 0.1

      window.addEventListener( 'resize', this.onWindowResize )

      document.body.onscroll = this.scrolling
      this.scrolling()
    },

    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize( window.innerWidth, window.innerHeight );
    },



    scrolling(){
      const t = document.body.getBoundingClientRect().top;


      this.camera.position.z = t * 2;
      this.camera.position.x = t * -0.002;
      this.camera.rotation.y = t * -0.002;


    },


    animate() {
      requestAnimationFrame(this.animate)
      this.render()
    },


    render() {

      // const time = performance.now() * 0.001;

      // this.mesh.position.y = Math.sin( time ) * 20 + 5;
      // this.mesh.rotation.x = time * 0.5;
      // this.mesh.rotation.z = time * 0.51;

      this.water.material.uniforms.time.value += 1.0 / 60.0;

      this.controls.update( this.clock.getDelta() )

      this.renderer.render( this.scene, this.camera );

    },


  }
}
</script>

<style scoped>

  

  main {
    width: 100vw;
    color: white;
    z-index: 99;
    position: absolute;
    width: 100%;
    margin: 0px auto;
    padding: 120px 0px;
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    top: 0;
    left: 0;
  }

  h1, h2, h3, blockquote {
    font-family: elevon, sans-serif;
    font-weight: 700;
    font-style: normal;
  }

  canvas {
    position: fixed;
    top: 0;
    left: 0;
  }



    header {
      background: var(--dark-bg);
      grid-column: 2 / span 5;
      font-size: 2.5rem;
      padding: 2rem;
      margin-bottom: var(--spacing);
    }

    section {
      grid-column: 2 / 8;
      padding: 1rem;
      background: var(--dark-bg);
      font-size: 1.25rem;
      line-height: 1.5;
      margin-bottom: var(--spacing);
    }

    blockquote {
      margin: 0;
      padding: 0;
      grid-column: 2 / span 9;
      margin-bottom: var(--spacing);
    }

    blockquote p {
      color: black;
      background-color: white;
      font-size: 4rem;
      display: inline;
      line-height: 1;
    }

    .left {
      grid-column: 6 / 12;
    }

</style>