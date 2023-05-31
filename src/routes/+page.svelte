<script lang="ts">
  import { Pane } from "tweakpane";
  import { browser } from "$app/environment";

  import * as Therelte from "@threlte/core";
  import * as Three from "three";
  import * as Utils from "three/src/math/MathUtils";

  const gridHelper = new Three.GridHelper(20, 10);
  const axesHelper = new Three.AxesHelper(10);

  const sphere = {
    position: { x: 0, y: 4, z: 0 },
  };

  if (browser) {
    const pane = new Pane({ title: "Scene" });

    const sphereControls = pane.addFolder({ title: "Sphere" });
    sphereControls.addInput(sphere, "position");

    sphereControls.on("change", ({ value }) => {
      sphere.position = value as any;
    });
  }
</script>

<Therelte.Canvas>
  <Therelte.Object3DInstance object={gridHelper} />
  <Therelte.Object3DInstance object={axesHelper} />
  <Therelte.PerspectiveCamera position={{ x: 20, y: 20, z: 20 }} fov={50}>
    <Therelte.OrbitControls autoRotate />
  </Therelte.PerspectiveCamera>

  <Therelte.AmbientLight color="white" intensity={0.2} />

  <Therelte.DirectionalLight
    color="white"
    position={{ x: 10, y: 20 }}
    intensity={2}
    shadow={{
      camera: { top: 10, bottom: -10, left: -10, right: 10 },
    }}
  />

  <Therelte.Mesh
    geometry={new Three.SphereGeometry(4, 64, 64)}
    material={new Three.MeshStandardMaterial({ color: "#F98269" })}
    position={sphere.position}
    receiveShadow
    castShadow
  />

  <Therelte.Mesh
    geometry={new Three.PlaneGeometry(20, 20)}
    material={new Three.MeshStandardMaterial({
      color: "#BD2201",
      side: Three.DoubleSide,
    })}
    rotation={{ x: Utils.DEG2RAD * 90 }}
    receiveShadow
  />
</Therelte.Canvas>
