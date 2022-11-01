

<template>
  <canvas id="container"></canvas>
</template>


<script>
  import * as THREE from 'three';
  import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
	import { TransformControls } from 'three/addons/controls/TransformControls.js';
  
  export default {
    name: "c",
    data: function() {
      return {
      };
    },
    mounted() {
      this.renderer;
      this.scene;
      this.currentCamera;

      this.init();
      this.render();
    },
    methods: {
      init() {
        let cameraPersp, cameraOrtho;
			  let control, orbit;

        let canvas = document.querySelector('#container')
        this.renderer = new THREE.WebGLRenderer({ canvas, antialias: true })
        this.renderer.shadowMap.enabled = true
        // this.renderer = new THREE.WebGLRenderer();
				// this.renderer.setPixelRatio( window.devicePixelRatio );
				// this.renderer.setSize( window.innerWidth, window.innerHeight );
				// document.body.appendChild( this.renderer.domElement );

				const aspect = window.innerWidth / window.innerHeight;

				cameraPersp = new THREE.PerspectiveCamera( 50, aspect, 0.01, 30000 );
				cameraOrtho = new THREE.OrthographicCamera( - 600 * aspect, 600 * aspect, 600, - 600, 0.01, 30000 );
				this.currentCamera = cameraPersp;

				this.currentCamera.position.set( 1000, 500, 1000 );
				this.currentCamera.lookAt( 0, 200, 0 );

				this.scene = new THREE.Scene();
        
        this.scene.background = new THREE.Color( 0xffffff );

				const light = new THREE.DirectionalLight( 0x808080, 2 );
				light.position.set( 1, 1, 1 );
				this.scene.add( light );

				orbit = new OrbitControls( this.currentCamera, this.renderer.domElement );
				orbit.update();
				orbit.addEventListener( 'change', this.render );

				control = new TransformControls( this.currentCamera, this.renderer.domElement );
				control.addEventListener( 'change', this.render );
				control.addEventListener( 'dragging-changed', function ( event ) {
					orbit.enabled = ! event.value;
				} );

        const geometry = new THREE.PlaneGeometry( 200, 200 );
        const material = new THREE.MeshNormalMaterial( { antialias: true } );
        material.side = THREE.DoubleSide;
        const plane1 = new THREE.Mesh( geometry, material );
        plane1.lookAt(new THREE.Vector3(0, 1, 0));
        plane1.position.add(new THREE.Vector3(-100, 0, 0));
        this.scene.add( plane1 );

        const plane2 = new THREE.Mesh( geometry, material );
        plane2.lookAt(new THREE.Vector3(1, 1, 0));
        plane2.position.add(new THREE.Vector3(70, -70, 0));
        this.scene.add( plane2 );

				const geometry_box = new THREE.BoxGeometry( 30, 30, 30 );
				const material_box = new THREE.MeshLambertMaterial( { transparent: true } );
				const box = new THREE.Mesh( geometry_box, material_box );
        box.position.add( new THREE.Vector3(15, 15, 0) );
				this.scene.add( box );

        const arrow1 = new THREE.ArrowHelper(
          new THREE.Vector3(1, 0, 0),
          new THREE.Vector3(0, 0, 0),
          100,
          0xff00ff
        );
        this.scene.add( arrow1 );

        const arrow2 = new THREE.ArrowHelper(
          (new THREE.Vector3(1, -1, 0)).normalize(),
          new THREE.Vector3(0, 0, 0),
          100,
          0x00ffff
        );
        this.scene.add( arrow2 );

				control.attach( box );
				this.scene.add( control );

			},
			render() {
				this.renderer.render( this.scene, this.currentCamera );
			}
    }
  };
</script>