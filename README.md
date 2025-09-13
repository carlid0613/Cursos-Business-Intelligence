<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Business Intelligence | Transformación Data-Driven</title>
    <link href="https://fonts.googleapis.com/css2?family=SF+Pro+Display:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --apple-black: #1d1d1f;
            --apple-dark-gray: #333336;
            --apple-gray: #86868b;
            --apple-light-gray: #f5f5f7;
            --apple-blue: #0071e3;
            --apple-green: #34c759;
            --apple-red: #ff3b30;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, sans-serif;
            background-color: var(--apple-black);
            color: white;
            overflow: hidden;
            height: 100vh;
            -webkit-font-smoothing: antialiased;
        }
        
        .presentation-container {
            width: 100vw;
            height: 100vh;
            position: relative;
            perspective: 1200px;
        }
        
        .slide {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 80px;
            text-align: center;
            opacity: 0;
            transition: all 1.2s cubic-bezier(0.42, 0, 0.05, 1);
            transform-style: preserve-3d;
            pointer-events: none;
        }
        
        .slide.active {
            opacity: 1;
            pointer-events: all;
        }
        
        .slide.next {
            transform: translateX(30%) scale(0.9);
            opacity: 0;
        }
        
        .slide.prev {
            transform: translateX(-30%) scale(0.9);
            opacity: 0;
        }
        
        .slide-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            z-index: -1;
            filter: brightness(0.4);
            transition: filter 1.2s ease;
        }
        
        .slide.active .slide-bg {
            filter: brightness(0.6);
        }
        
        .content {
            max-width: 900px;
            margin: 0 auto;
        }
        
        h1 {
            font-size: 5.5rem;
            font-weight: 700;
            margin-bottom: 2rem;
            line-height: 1.1;
            text-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            opacity: 0;
            transform: translateY(50px);
            transition: all 0.8s cubic-bezier(0.215, 0.61, 0.355, 1);
        }
        
        h2 {
            font-size: 2.2rem;
            font-weight: 500;
            margin-bottom: 3rem;
            color: var(--apple-ligth-gray);
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
            opacity: 0;
            transform: translateY(50px);
            transition: all 0.8s cubic-bezier(0.215, 0.61, 0.355, 1) 0.1s;
        }
        
        p {
            font-size: 1.6rem;
            line-height: 1.5;
            margin-bottom: 3rem;
            font-weight: 300;
            opacity: 0;
            transform: translateY(50px);
            transition: all 0.8s cubic-bezier(0.215, 0.61, 0.355, 1) 0.2s;
        }
        
        .highlight {
            color: var(--apple-green);
            font-weight: 500;
        }
        
        .problem {
            color: var(--apple-black);
            background-color: rgba(200, 200, 200, 0.5); /* gris claro semi-transparente */
            padding: 2px 4px; /* espacio alrededor para que se vea mejor */
            border-radius: 3px; /* bordes suavizados */
        }
        
        .solution {
            color: var(--apple-green);
        }
        
        .cta-button {
            background: linear-gradient(135deg, var(--apple-blue), #0066cc);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.4rem;
            border-radius: 30px;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 2rem;
            text-decoration: none;
            display: inline-block;
            font-weight: 500;
            opacity: 0;
            transform: translateY(50px);
            transition: all 0.8s cubic-bezier(0.215, 0.61, 0.355, 1) 0.3s;
            box-shadow: 0 4px 20px rgba(0, 113, 227, 0.3);
        }
        
        .cta-button:hover {
            background: linear-gradient(135deg, #0066cc, var(--apple-blue));
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 8px 25px rgba(0, 113, 227, 0.4);
        }
        
        .slide.active h1,
        .slide.active h2,
        .slide.active p,
        .slide.active .cta-button {
            opacity: 1;
            transform: translateY(0);
        }
        
        .image-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin: 3rem 0;
            opacity: 0;
            transform: scale(0.9);
            transition: all 1s cubic-bezier(0.215, 0.61, 0.355, 1) 0.4s;
        }
        
        .slide.active .image-grid {
            opacity: 1;
            transform: scale(1);
        }
        
        .image-grid img {
            width: 100%;
            height: 200px;
            object-fit: cover;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            transition: all 0.5s ease;
        }
        
        .image-grid img:hover {
            transform: scale(1.03);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.4);
        }
        
        .progress-dots {
            position: fixed;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 12px;
            z-index: 100;
        }
        
        .dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            background-color: rgba(255, 255, 255, 0.3);
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .dot.active {
            background-color: white;
            transform: scale(1.3);
        }
        
        /* Efectos especiales */
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }
        
        .floating {
            animation: float 6s ease-in-out infinite;
        }
        
        /* Transición entre slides */
        .slide-exit {
            transform: translateX(-100%) rotateY(-20deg) scale(0.9);
            opacity: 0;
        }
        
        .slide-enter {
            transform: translateX(100%) rotateY(20deg) scale(0.9);
            opacity: 0;
        }
        
        .slide-enter-active {
            transform: translateX(0) rotateY(0) scale(1);
            opacity: 1;
        }
    </style>
</head>
<body>
    <div class="presentation-container">
        <!-- Slide 1: Hero -->
        <div class="slide active" id="slide1">
            <img src="https://images.unsplash.com/photo-1532619675605-1ede6c2ed2b0?q=80&w=1170&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D" class="slide-bg">
            <div class="content">
                <h1>Business Intelligence</h1>
                <h2>Descubre el poder de tus datos</h2>
                <p>Donde otros ven <span class="problem">caos,</span> tú puedes 
                el <span class="solution">ver patrones, soluciones y oportunidades</span> Entra al mundo de Business Intelligence y evoluciona tu carrera.</p>
                <!-- <a href="#" class="cta-button">Explora el curso</a>-->
            </div>
        </div>
        
        <!-- Slide 2: El problema -->
        <div class="slide" id="slide2">
            <img src="https://i.pinimg.com/736x/a6/6f/09/a66f0935dd8d65820ee50d7e67c9f32b.jpg" class="slide-bg">
            <div class="content">
                <h1>El problema real</h1>
                <h2>Decisiones basadas en intuición, no en datos</h2>
                <div class="image-grid">
                    <img src="https://i.pinimg.com/736x/8c/73/df/8c73df77c8d650e553a7ee1986542f07.jpg" alt="Datos confusos">
                    <img src="https://i.pinimg.com/736x/9d/f9/f7/9df9f7317c8edf2bd638ba7fa6626248.jpg" alt="Frustración">
                    <img src="https://i.pinimg.com/736x/b7/b3/fd/b7b3fd7e14e0dfa7b8bd98fe13f54566.jpg" alt="Gráficos confusos">
                </div>
                <p>En un mundo donde <span class="problem"><strong>el exceso de información abruma y el 86% de empresas toman decisiones críticas</strong> </span>  sin el soporte adecuado de datos.</p>
            </div>
        </div>
        
        <!-- Slide 3: Consecuencias -->
        <div class="slide" id="slide3">
            <img src="https://images.unsplash.com/photo-1497366754035-f200968a6e72?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2400&q=80" class="slide-bg">
            <div class="content">
                <h1>El costo del caos</h1>
                <h2>Tu carrera se estanca por no saber cómo aprovechar el poder de los datos</h2>
                <div class="image-grid">
                    <img src="https://media.licdn.com/dms/image/v2/C4D12AQHEaSY-O7GXjQ/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1618795972776?e=2147483647&v=beta&t=Sl6Hp62BYJf5yBvsW9L4ED04VKYQTcstXSWtApOmSAE" alt="Pérdidas financieras">
                    <img src="https://www.hubspot.com/hubfs/media/entrevistatrabajo.jpeg" alt="Competencia">
                    <img src="https://www.caritas.org.mx/wp-content/uploads/2020/08/consecuencias-del-desempleo-1024x768.jpg" alt="Oportunidades perdidas">
                </div>
                <p>No saber Excel ni herramientas de BI no solo <span class="problem">limitan tus habilidades </span> de obtener mejores puestos de trabajo y salarios competitivos. <span class="problem">Mientras las empresas sin BI fallan y son menos eficientes</span> los profesionales que sí dominan estas herramientas se vuelven piezas claves.</p>
            </div>
        </div>
        
        <!-- Slide 4: La solución -->
        <div class="slide" id="slide4">
            <img src="https://i.pinimg.com/736x/77/ed/8c/77ed8c4f912345f77a151fbb99a5f141.jpg" class="slide-bg">
            <div class="content">
                <h1>La solución</h1>
                <h2>Domina el arte del Business Intelligence</h2>
                <div class="image-grid">
                    <img src="https://images.unsplash.com/photo-1460925895917-afdab827c52f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Dashboard">
                    <img src="https://images.unsplash.com/photo-1504868584819-f8e8b4b6d7e3?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Visualización de datos">
                    <img src="https://images.unsplash.com/photo-1526628953301-3e589a6a8b74?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Análisis">
                </div>
                <p>Aquí comienza tu transformación profesional <span class="solution">Domina Excel, Power Bi entre otras herramientas. </span> El camino para ser un experto en <span class="solution">análisis de datos, automatización y visualización de datos.</span>.</p>
                <a  class="cta-button">Usa los datos a tu favor y prepara un futuro más sólido, con mejores oportunidades laborales y con un salario competitivo</a>
            </div>
        </div>
        
        <!-- Slide 5: Beneficios -->
        <div class="slide" id="slide5">
            <img src="https://images.unsplash.com/photo-1504868584819-f8e8b4b6d7e3?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2400&q=80" class="slide-bg">
            <div class="content">
                <h1>Resultados tangibles</h1>
                <h2>Lo que lograrás con los cursos</h2>
                <div class="image-grid">
                    <img src="https://images.unsplash.com/photo-1521791055366-0d553872125f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Toma de decisiones">
                    <img src="https://images.unsplash.com/photo-1522071820081-009f0129c71c?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Equipo trabajando">
                    <img src="https://images.unsplash.com/photo-1542744173-8e7e53415bb0?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Gráficos claros">
                </div>
                <p>Aprenderás teoría, <span class="solution">Dominio práctico de Excel </span> y <span class="solution"> herramientas de BI </span> para resolver problemas reales.</p>
            </div>
        </div>
        
        <!-- Slide 6: CTA -->
        <div class="slide" id="slide6">
            <img src="https://images.unsplash.com/photo-1522202176988-66273c2fd55f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2400&q=80" class="slide-bg">
            <div class="content">
                <h1>Tu próximo paso</h1>
                <h2>Transforma datos en ventaja competitiva</h2>
                <div class="image-grid">
                    <img src="https://images.unsplash.com/photo-1579389083078-4e7018379f7e?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Estudiante feliz">
                    <img src="https://images.unsplash.com/photo-1523240795612-9a054b0db644?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Certificación">
                    <img src="https://images.unsplash.com/photo-1521791055366-0d553872125f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80" alt="Éxito">
                </div>
                <p>Únete a la próxima generación de líderes <span class="solution">data-driven</span>.</p>
                <a href="https://sites.google.com/view/business-intelligence-2024/cursos" class="cta-button">Explora los Cursos</a>
            </div>
        </div>
        
        <div class="progress-dots">
            <div class="dot active" data-slide="0"></div>
            <div class="dot" data-slide="1"></div>
            <div class="dot" data-slide="2"></div>
            <div class="dot" data-slide="3"></div>
            <div class="dot" data-slide="4"></div>
            <div class="dot" data-slide="5"></div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const slides = document.querySelectorAll('.slide');
            const dots = document.querySelectorAll('.dot');
            let currentSlide = 0;
            const totalSlides = slides.length;
            let slideInterval;
            const slideDuration = 10000; // 10 segundos por slide
            
            // Iniciar presentación
            startSlideShow();
            
            function startSlideShow() {
                slideInterval = setInterval(nextSlide, slideDuration);
            }
            
            function nextSlide() {
                goToSlide((currentSlide + 1) % totalSlides);
            }
            
            function goToSlide(n) {
                // Actualizar clase del slide actual
                slides[currentSlide].classList.remove('active');
                slides[currentSlide].classList.add('prev');
                
                // Actualizar puntos de navegación
                dots[currentSlide].classList.remove('active');
                
                currentSlide = (n + totalSlides) % totalSlides;
                
                // Aplicar clases al nuevo slide
                slides[currentSlide].classList.remove('prev', 'next');
                slides[currentSlide].classList.add('active');
                dots[currentSlide].classList.add('active');
                
                // Reiniciar temporizador
                clearInterval(slideInterval);
                startSlideShow();
            }
            
            // Navegación por puntos
            dots.forEach((dot, index) => {
                dot.addEventListener('click', () => {
                    goToSlide(index);
                });
            });
            
            // Navegación por teclado
            document.addEventListener('keydown', (e) => {
                if (e.key === 'ArrowRight') {
                    nextSlide();
                } else if (e.key === 'ArrowLeft') {
                    goToSlide(currentSlide - 1);
                }
            });
            
            // Navegación por swipe (para móviles)
            let touchStartX = 0;
            let touchEndX = 0;
            
            document.addEventListener('touchstart', (e) => {
                touchStartX = e.changedTouches[0].screenX;
            }, false);
            
            document.addEventListener('touchend', (e) => {
                touchEndX = e.changedTouches[0].screenX;
                handleSwipe();
            }, false);
            
            function handleSwipe() {
                if (touchEndX < touchStartX - 50) {
                    nextSlide();
                }
                if (touchEndX > touchStartX + 50) {
                    goToSlide(currentSlide - 1);
                }
            }
        });
    </script>
</body>
</html>