<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pato en el Botón - SVG Animado con Visor 3D</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* Ajustes básicos de estilos */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { font-family: Arial, sans-serif; overflow-y: auto; background: #f7f8fa; color: #333; }

        /* Encabezado */
        #header {
            position: fixed;
            top: 0;
            width: 100%;
            padding: 20px 40px;
            background: #f7f8fa;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            z-index: 1000;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        #header .logo { font-size: 24px; font-weight: bold; color: #333; }

        .header-content {
            display: flex;
            align-items: center;
            gap: 30px;
        }

        .header-text {
            font-size: 18px;
            color: #333;
        }

        .button-group { display: flex; align-items: center; gap: 15px; }

        /* Botones del encabezado */
        .header-button {
            padding: 10px 20px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-weight: bold;
            font-size: 14px;
            transition: background-color 0.3s, color 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .button-secondary {
            background-color: #e3e4e8;
            color: #333;
        }

        .button-secondary:hover {
            background-color: #ccc;
        }

        .button-primary {
            background-color: #333;
            color: #ffffff;
        }

        .button-primary:hover {
            background-color: #444;
        }

        .circle-icon {
            background-color: #e3e4e8;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            transition: background-color 0.3s;
            border: 2px solid black;
        }

        svg {
            width: 50px;
            height: 50px;
        }

        .duck-path {
            fill: none;
            stroke: #333;
            stroke-width: 5;
            stroke-linecap: round;
            stroke-linejoin: round;
            stroke-dasharray: 1000;
            stroke-dashoffset: 1000;
        }

        /* Clase de animación para el pato */
        .animate-duck {
            animation: drawDuck 5s ease-in-out infinite;
        }

        /* Keyframes para la animación */
        @keyframes drawDuck {
            0% {
                stroke-dashoffset: 1000;
            }
            50% {
                stroke-dashoffset: 0;
            }
            100% {
                stroke-dashoffset: 1000;
            }
        }

        /* Menú desplegable 1 */
        .menu-dropdown {
            position: absolute;
            top: 105px; /* Ajusta esta posición para colocar el menú 1 */
            right: 40px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
            display: none;
            flex-direction: column;
            padding: 10px;
            width: 180px;
            background-color: #ffffff;
            box-sizing: border-box;
        }

        /* Menú desplegable 2 */
        .menu-dropdown2 {
            position: absolute;
            top: 240px; /* Ajusta esta posición para colocar el menú 2 más abajo */
            right: 40px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
            display: none;
            flex-direction: column;
            padding: 10px;
            width: 180px;
            background-color: #333333;
            box-sizing: border-box;
        }

        /* Estilos para enlaces de ambos menús */
        .menu-dropdown a, .menu-dropdown2 a {
            text-decoration: none;
            padding: 8px 10px;
            font-size: 18px;
            transition: color 0.3s ease, background-color 0.3s ease;
            border-radius: 5px;
        }

        .menu-dropdown a {
            color: #333;
        }

        .menu-dropdown a:hover {
            color: #0096ff;
            background-color: #f0f0f0;
        }

        .menu-dropdown2 a {
            color: #ffffff;
        }

        .menu-dropdown2 a:hover {
            color: #0096ff;
            background-color: #555555;
        }

        /* Contenedor para el canvas 3D */
        #three-container {
            margin-top: 600px;
            width: 1200px;
            height: 600px;
            position: relative;
            left: 20px;
        }

        canvas {
            width: 100%;
            height: 100%;
        }
    </style>
</head>
<body>
    <div id="header">
        <div class="logo">Karla Rosario</div>
        <div class="header-content">
            <div class="header-text">
                We help brands create digital experiences that connect with their audience
            </div>
            <div class="button-group">
                <div id="circle-icon" class="circle-icon" onclick="toggleDuckAnimation()">
                    <svg viewBox="0 0 100 100">
                        <path id="duck-path" class="duck-path" d="M 20 70 Q 20 50, 40 50 Q 35 30, 55 30 Q 75 30, 75 50 Q 85 55, 75 70 Q 60 90, 30 80 Q 20 75, 20 70 Z M 60 40 A 5 5 0 1 1 59.9 40 Z M 70 50 Q 85 47, 85 50 Q 85 53, 70 50 Z" />
                    </svg>
                </div>
                <button class="header-button button-primary" onclick="letsTalk()"><i class="fas fa-comment-dots"></i> LET'S TALK</button>
                <button id="menu-button" class="header-button button-secondary" onclick="toggleMenu()"><i class="fas fa-ellipsis-h"></i> MENU</button>
            </div>
        </div>
    </div>

    <!-- Menús desplegables debajo del encabezado -->
    <div id="menu-container" class="menu-dropdown">
        <a href="#">HOME</a>
        <a href="#">ABOUT ME</a>
        <a href="#">CONTACT</a>
    </div>
    <div id="menu2-container" class="menu-dropdown2">
        <a href="#">PROJECTS</a>
    </div>

    <!-- Contenedor para el visor 3D -->
    <div id="three-container"></div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/GLTFLoader.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/libs/draco/draco_decoder.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/DRACOLoader.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
    <script>
        let scene, camera, renderer, model, controls, autoRotate = true, lastMouseDirection = 0.01;

        function init() {
            // Configuración de la escena
            scene = new THREE.Scene();

            // Configuración de la cámara
            camera = new THREE.PerspectiveCamera(50, 2, 0.1, 1000);
            camera.position.z = 5;

            // Configuración del renderizador
            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setSize(1200, 600);
            renderer.setClearColor(0x000000, 0); // Fondo transparente
            document.getElementById('three-container').appendChild(renderer.domElement);

            // Controles de órbita para rotar el modelo
            controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true; // suaviza la interacción con el mouse
            controls.dampingFactor = 0.05;
            controls.screenSpacePanning = false;
            controls.maxPolarAngle = Math.PI / 2;

            // Detectar cuando el usuario hace clic o deja de hacer clic
            controls.addEventListener('start', () => {
                autoRotate = false;
            });

            controls.addEventListener('end', () => {
                lastMouseDirection = controls.getAzimuthalAngle() > 0 ? 0.01 : -0.01;
                autoRotate = true;
            });

            // Cargar Draco y GLTFLoader
            const dracoLoader = new THREE.DRACOLoader();
            dracoLoader.setDecoderPath('https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/libs/draco/');

            const loader = new THREE.GLTFLoader();
            loader.setDRACOLoader(dracoLoader);

            // Carga del modelo GLB
            loader.load(
                'Lámpara.glb', // Ruta del archivo Lámpara.glb
                function(gltf) {
                    model = gltf.scene;

                    // Ajustar la escala del modelo para hacerlo más pequeño
                    model.scale.set(0.2, 0.2, 0.2); // Ajuste base de la escala

                    scene.add(model);
                },
                undefined,
                function(error) {
                    console.error(error);
                }
            );

            // Luz
            const light = new THREE.DirectionalLight(0xffffff, 1);
            light.position.set(0, 1, 1).normalize();
            scene.add(light);

            // Rotación automática y renderizado
            function animate() {
                requestAnimationFrame(animate);
                if (model && autoRotate) {
                    model.rotation.y += lastMouseDirection; // Gira en la última dirección usada por el mouse
                }
                controls.update(); // Actualizar los controles de órbita
                renderer.render(scene, camera);
            }
            animate();

            // Ajuste del tamaño de la ventana
            window.addEventListener('resize', function() {
                const container = document.getElementById('three-container');
                renderer.setSize(container.clientWidth, container.clientHeight);
                camera.aspect = container.clientWidth / container.clientHeight;
                camera.updateProjectionMatrix();
            });
        }

        init();

        let isAnimating = false;

        function toggleDuckAnimation() {
            const duckPath = document.getElementById('duck-path');
            const circleIcon = document.getElementById('circle-icon');
            if (isAnimating) {
                circleIcon.style.backgroundColor = '#e3e4e8';
                duckPath.classList.remove('animate-duck'); // Elimina la animación
                isAnimating = false;
            } else {
                circleIcon.style.backgroundColor = 'yellow';
                duckPath.classList.add('animate-duck'); // Añade la animación
                isAnimating = true;
            }
        }

        function toggleMenu() {
            const menuContainer = document.getElementById('menu-container');
            const menu2Container = document.getElementById('menu2-container');
            const isMenuVisible = menuContainer.style.display === 'flex';

            menuContainer.style.display = isMenuVisible ? 'none' : 'flex';
            menu2Container.style.display = isMenuVisible ? 'none' : 'flex';
        }

        function letsTalk() {
            alert("Let's talk clicked!");
        }
    </script>
</body>
</html>
