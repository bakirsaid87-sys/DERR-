# DERRبimport React, { useState, useEffect } from 'react';
import { Canvas } from '@react-three/fiber';
import { OrbitControls, useGLTF } from '@react-three/drei';
import * as THREE from 'three';

// Component for loading 3D models
function Model({ url }) {
  const { scene } = useGLTF(url);
  return <primitive object={scene} />;
}

// Main App Component
function App() {
  const [phase, setPhase] = useState('brand'); // 'brand', 'products', 'customize'
  const products = ['shirt.gltf', 'pants.gltf', 'dress.gltf']; // Example product URLs
  const [currentProduct, setCurrentProduct] = useState(0);

  // Auto-transition logic
  useEffect(() => {
    if (phase === 'brand') {
      setTimeout(() => setPhase('products'), 5000); // 5s delay
    } else if (phase === 'products') {
      const interval = setInterval(() => {
        setCurrentProduct((prev) => (prev + 1) % products.length);
      }, 3000); // Cycle every 3s
      return () => clearInterval(interval);
    }
  }, [phase, products.length]);

  return (
    <div style={{ height: '100vh' }}>
      <Canvas camera={{ position: [0, 0, 5] }}>
        <ambientLight intensity={0.5} />
        <pointLight position={[10, 10, 10]} />
        <OrbitControls enableZoom={false} />

        {phase === 'brand' && (
          // Immersive brand scene: e.g., floating logo and particles
          <>
            <mesh>
              <boxGeometry args={[1, 1, 1]} />
              <meshStandardMaterial color="gold" />
            </mesh>
            {/* Add more 3D elements for high-fashion feel */}
          </>
        )}

        {phase === 'products' && (
          // Display products one by one
          <Model url={products[currentProduct]} />
        )}

        {phase === 'customize' && (
          // Placeholder for customization UI (overlay)
          <div style={{ position: 'absolute', top: 0, right: 0, background: 'white', padding: '20px' }}>
            <h3>Admin Panel</h3>
            <input type="file" placeholder="Upload Logo" />
            <input type="text" placeholder="Add Product URL" />
            <button onClick={() => alert('Changes saved!')}>Save</button>
          </div>
        )}
      </Canvas>

      {/* Navigation buttons for testing */}
      <button onClick={() => setPhase('brand')}>Brand</button>
      <button onClick={() => setPhase('products')}>Products</button>
      <button onClick={() => setPhase('customize')}>Customize</button>
    </div>
  );
}

export default App;
