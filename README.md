<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tu Viaje con Maz | Constructora</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700&family=Playfair+Display:ital,wght@0,600;1,700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --maz-red: #c66b6b;
            --maz-gold: #b38e5d;
            --maz-bg: #fdf5f2;
        }
        
        body {
            background-color: var(--maz-bg);
            color: #4a4a4a;
            font-family: 'Montserrat', sans-serif;
            overflow-x: hidden;
            scroll-behavior: smooth;
        }

        .font-serif-maz {
            font-family: 'Playfair Display', serif;
        }

        .maz-logo {
            filter: invert(36%) sepia(21%) saturate(1142%) hue-rotate(314deg) brightness(96%) contrast(89%);
            max-width: 280px;
            transition: transform 0.3s ease;
        }

        .cloud {
            position: absolute;
            opacity: 0.25;
            pointer-events: none;
            z-index: 0;
            animation: moveClouds 25s linear infinite;
        }

        @keyframes moveClouds {
            from { transform: translateX(-15vw); }
            to { transform: translateX(100vw); }
        }

        .flight-path {
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            width: 4px;
            height: 100%;
            border-left: 3px dashed rgba(198, 107, 107, 0.3);
            z-index: 0;
        }

        .flight-path-fill {
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            width: 3px;
            background: linear-gradient(to bottom, transparent, var(--maz-red));
            z-index: 1;
            top: 0;
            height: 0;
            transition: height 0.1s linear;
        }

        .plane-cursor {
            position: absolute;
            left: 50%;
            transform: translateX(-50%) rotate(180deg);
            color: var(--maz-red);
            font-size: 28px;
            z-index: 20;
            transition: top 0.1s linear;
            filter: drop-shadow(0px 4px 10px rgba(198, 107, 107, 0.5));
        }

        .step-card {
            background: white;
            border-radius: 24px;
            transition: all 0.4s cubic-bezier(0.165, 0.84, 0.44, 1);
            border: 1px solid rgba(198, 107, 107, 0.12);
            position: relative;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(198, 107, 107, 0.04);
        }

        .step-card::before {
            content: "PASAPORTE MAZ";
            position: absolute;
            top: 15px;
            right: -35px;
            background: var(--maz-red);
            color: white;
            font-size: 9px;
            padding: 6px 45px;
            transform: rotate(45deg);
            font-weight: bold;
            letter-spacing: 1px;
        }

        .step-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 25px 50px rgba(198, 107, 107, 0.1);
            border-color: var(--maz-red);
        }

        .image-window {
            border-radius: 40px 10px 40px 10px;
            overflow: hidden;
            position: relative;
            box-shadow: 0 20px 40px rgba(0,0,0,0.08);
            transition: all 0.5s ease;
        }
        
        .image-window:hover {
            border-radius: 10px 40px 10px 40px;
        }

        .step-number {
            background: var(--maz-red);
            width: 48px;
            height: 48px;
            border-radius: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            box-shadow: 4px 4px 0px rgba(198, 107, 107, 0.25);
        }

        .icon-box {
            width: 70px;
            height: 70px;
            background: var(--maz-bg);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 20px;
            color: var(--maz-red);
            font-size: 1.75rem;
            border: 2px solid white;
            box-shadow: 0 8px 20px rgba(0,0,0,0.03);
        }

        @media (max-width: 768px) {
            .flight-path { left: 24px; transform: none; }
            .flight-path-fill { left: 24px; transform: none; }
            .plane-cursor { left: 24px; transform: rotate(180deg); }
        }
    </style>
</head>
<body>

    <nav class="fixed w-full z-50 bg-white/80 backdrop-blur-md border-b border-gray-100 py-3">
        <div class="max-w-6xl mx-auto px-6 flex justify-between items-center">
            <img src="https://maz.com.co/wp-content/uploads/2022/07/maz-logo.svg" alt="Maz Logo" class="h-8 maz-logo">
            <button onclick="toggleModal()" class="bg-[#c66b6b] text-white text-xs px-5 py-2.5 rounded-full font-bold uppercase tracking-wider hover:bg-red-800 transition-colors shadow-md">Check-in Online</button>
        </div>
    </nav>

    <div id="checkinModal" class="fixed inset-0 z-50 hidden bg-black/60 backdrop-blur-sm flex items-center justify-center p-4 transition-all duration-300">
        <div class="bg-white rounded-3xl max-w-md w-full p-8 shadow-2xl relative border border-red-100 transform scale-95 transition-transform duration-300">
            <button onclick="toggleModal()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-xl"><i class="fas fa-times"></i></button>
            <div class="text-center mb-6">
                <div class="w-16 h-16 bg-[#fdf5f2] rounded-full flex items-center justify-center mx-auto mb-3 text-[#c66b6b] text-2xl"><i class="fas fa-plane-departure"></i></div>
                <h3 class="text-2xl font-bold text-gray-800 font-serif-maz">Pre-Check-in Digital</h3>
                <p class="text-xs text-gray-500 mt-1">Valida tu perfil y recibe atención prioritaria en minutos.</p>
            </div>
            <form class="space-y-4" onsubmit="event.preventDefault(); alert('¡Check-in exitoso! Un asesor premium de Maz se comunicará contigo.'); toggleModal();">
                <div>
                    <label class="block text-xs font-bold uppercase text-gray-600 mb-1 tracking-wider">Nombre Completo</label>
                    <input type="text" required class="w-full bg-gray-50 border border-gray-200 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-[#c66b6b]" placeholder="Ej. Juan Pérez">
                </div>
                <div>
                    <label class="block text-xs font-bold uppercase text-gray-600 mb-1 tracking-wider">¿Cuál es tu destino de interés?</label>
                    <select class="w-full bg-gray-50 border border-gray-200 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-[#c66b6b]">
                        <option>Bogotá Centro (Inversión)</option>
                        <option>Afflora (Barranquilla)</option>
                        <option>Maramá (Villavicencio)</option>
                        <option>Allegro (Cartago)</option>
                        <option>More (Tocancipá)</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-bold uppercase text-gray-600 mb-1 tracking-wider">¿Cuentas con subsidio de vivienda?</label>
                    <div class="flex gap-4 mt-1">
                        <label class="flex items-center gap-2 text-sm text-gray-600 cursor-pointer"><input type="radio" name="subsidio" class="accent-[#c66b6b]"> Sí, Mi Casa Ya / Caja</label>
                        <label class="flex items-center gap-2 text-sm text-gray-600 cursor-pointer"><input type="radio" name="subsidio" class="accent-[#c66b6b]"> No por ahora</label>
                    </div>
                </div>
                <button type="submit" class="w-full bg-[#c66b6b] text-white py-3.5 rounded-xl font-bold text-xs uppercase tracking-widest hover:bg-red-800 transition-colors mt-2 shadow-lg shadow-red-100">CONFIRMAR ABORDAJE</button>
            </form>
        </div>
    </div>

    <header class="relative min-h-screen flex items-center justify-center pt-20 overflow-hidden">
        <i class="fas fa-cloud cloud text-white text-7xl" style="top: 18%; left: 8%;"></i>
        <i class="fas fa-cloud cloud text-white text-9xl" style="top: 35%; left: 75%; animation-delay: 3s;"></i>
        
        <div class="max-w-4xl px-6 text-center z-10">
            <span class="text-[#c66b6b] font-bold tracking-[0.4em] uppercase text-xs md:text-sm mb-4 block">Tu próximo destino: La felicidad</span>
            <h1 class="text-4xl md:text-6xl font-light italic text-gray-800 leading-tight">Empieza tu viaje hacia el <br> <span class="font-bold not-italic font-serif-maz text-[#c66b6b]">hogar de tus sueños</span></h1>
            
            <div class="my-10">
                <img src="https://maz.com.co/wp-content/uploads/2022/07/maz-logo.svg" alt="Maz Logo" class="maz-logo mx-auto w-64 md:w-80">
            </div>

            <p class="text-lg md:text-xl text-gray-500 italic mb-10 max-w-2xl mx-auto">"No solo construimos proyectos, diseñamos el itinerario para la aventura más importante de tu vida."</p>
            
            <a href="#paso-01" class="inline-flex items-center gap-3 bg-[#c66b6b] text-white px-8 py-4 rounded-full font-bold shadow-2xl hover:bg-red-800 transition-all transform hover:scale-105">
                VER PLAN DE VUELO <i class="fas fa-plane"></i>
            </a>
        </div>
    </header>

    <section id="viaje" class="relative py-20 px-6 max-w-6xl mx-auto">
        <div class="flight-path"></div>
        <div class="flight-path-fill" id="pathFill"></div>
        <div id="plane" class="plane-cursor"><i class="fas fa-plane"></i></div>

        <div id="paso-01" class="flex flex-col md:flex-row items-center justify-between mb-36 relative z-10 group/item scroll-mt-28">
            <div class="w-full md:w-5/12 step-card p-8 ml-12 md:ml-0">
                <div class="flex items-center gap-4 mb-6">
                    <div class="step-number">01</div>
                    <h3 class="text-lg md:text-xl font-bold text-gray-800 uppercase tracking-tighter">Prepara tu equipaje</h3>
                </div>
                <div class="icon-box group-hover/item:rotate-12 transition-transform duration-500"><i class="fas fa-luggage-cart"></i></div>
                <h4 class="text-lg font-bold text-[#c66b6b] mb-2">Elige tu destino</h4>
                <p class="text-gray-600 text-sm leading-relaxed">
                    Inicia esta aventura explorando nuestros proyectos y elige el lugar donde quieres construir tus próximos recuerdos. Nuestro equipo estará listo para darte una guía totalmente personalizada.
                </p>
            </div>
            <div class="hidden md:block w-2/12 text-center text-4xl text-[#c66b6b]/20 group-hover/item:text-[#c66b6b] transition-colors duration-500">
                <i class="fas fa-map-marked-alt"></i>
            </div>
            <div class="w-full md:w-5/12 mt-6 md:mt-0 ml-12 md:ml-0 image-window h-64 bg-gray-200">
                <img src="https://maz.com.co/wp-content/uploads/2024/09/more-maz-fachada-vista2-apartamento-tocancipa-min-optimized.jpg" alt="Proyecto More Maz" class="w-full h-full object-cover transform group-hover/item:scale-110 transition-transform duration-700 filter brightness-95">
                <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent flex items-end p-6">
                    <p class="text-white font-bold tracking-widest text-xs uppercase">Proyecto More - Tocancipá</p>
                </div>
            </div>
        </div>

        <div class="flex flex-col md:flex-row-reverse items-center justify-between mb-36 relative z-10 group/item">
            <div class="w-full md:w-5/12 step-card p-8 ml-12 md:ml-0">
                <div class="flex items-center gap-4 mb-6">
                    <div class="step-number">02</div>
                    <h3 class="text-lg md:text-xl font-bold text-gray-800 uppercase tracking-tighter">Reserva tu tiquete</h3>
                </div>
                <div class="icon-box group-hover/item:scale-110 transition-transform duration-500"><i class="fas fa-ticket-alt"></i></div>
                <h4 class="text-lg font-bold text-[#c66b6b] mb-2">Separa tu inmueble</h4>
                <p class="text-gray-600 text-sm leading-relaxed mb-4">
                    Es hora de asegurar tu lugar en el vuelo. Te guiaremos con los documentos necesarios de forma ágil y segura sin salir de casa, estructurados especialmente según tu perfil:
                </p>
                <div class="flex flex-wrap gap-2">
                    <span class="bg-[#fdf5f2] text-[#c66b6b] text-[11px] font-bold px-3 py-1 rounded-full border border-[#c66b6b]/20">Empleado</span>
                    <span class="bg-[#fdf5f2] text-[#c66b6b] text-[11px] font-bold px-3 py-1 rounded-full border border-[#c66b6b]/20">Independiente</span>
                    <span class="bg-[#fdf5f2] text-[#c66b6b] text-[11px] font-bold px-3 py-1 rounded-full border border-[#c66b6b]/20">Pensionado</span>
                </div>
            </div>
            <div class="hidden md:block w-2/12 text-center text-4xl text-[#c66b6b]/20 group-hover/item:text-[#c66b6b] transition-colors duration-500">
                <i class="fas fa-passport"></i>
            </div>
            <div class="w-full md:w-5/12 mt-6 md:mt-0 ml-12 md:ml-0 image-window h-64 bg-gray-200">
                <img src="https://maz.com.co/wp-content/uploads/2025/07/piscina-allegro-e3-apartamentos-cartago-maz-constructora-jul2025-optimized.jpg" alt="Piscina Allegro Maz" class="w-full h-full object-cover transform group-hover/item:scale-110 transition-transform duration-700 filter brightness-95">
                <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent flex items-end p-6">
                    <p class="text-white font-bold tracking-widest text-xs uppercase">Proyecto Allegro - Cartago</p>
                </div>
            </div>
        </div>

        <div class="flex flex-col md:flex-row items-center justify-between mb-36 relative z-10 group/item">
            <div class="w-full md:w-5/12 step-card p-8 ml-12 md:ml-0">
                <div class="flex items-center gap-4 mb-6">
                    <div class="step-number">03</div>
                    <h3 class="text-lg md:text-xl font-bold text-gray-800 uppercase tracking-tighter">Conecta con aliados</h3>
                </div>
                <div class="icon-box group-hover/item:-rotate-12 transition-transform duration-500"><i class="fas fa-network-wired"></i></div>
                <h4 class="text-lg font-bold text-[#c66b6b] mb-2">Radica tu crédito</h4>
                <p class="text-gray-600 text-sm leading-relaxed">
                    Este paso es como elegir la mejor aerolínea para optimizar tu viaje. Te acercamos de primera mano a nuestras entidades financieras aliadas con el fin de ayudarte a encontrar la ruta de financiación más cómoda y eficiente.
                </p>
            </div>
            <div class="hidden md:block w-2/12 text-center text-4xl text-[#c66b6b]/20 group-hover/item:text-[#c66b6b] transition-colors duration-500">
                <i class="fas fa-shield-heart"></i>
            </div>
            <div class="w-full md:w-5/12 mt-6 md:mt-0 ml-12 md:ml-0 image-window h-64 bg-gray-200">
                <img src="https://maz.com.co/wp-content/uploads/2022/07/cauttiva-fachada-optimized.webp" alt="Proyecto Cauttiva Maz" class="w-full h-full object-cover transform group-hover/item:scale-110 transition-transform duration-700 filter brightness-95">
                <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent flex items-end p-6">
                    <p class="text-white font-bold tracking-widest text-xs uppercase">Proyecto Cauttiva - Exclusividad</p>
                </div>
            </div>
        </div>

        <div class="flex flex-col md:flex-row-reverse items-center justify-between mb-36 relative z-10 group/item">
            <div class="w-full md:w-5/12 step-card p-8 ml-12 md:ml-0">
                <div class="flex items-center gap-4 mb-6">
                    <div class="step-number">04</div>
                    <h3 class="text-lg md:text-xl font-bold text-gray-800 uppercase tracking-tighter">Confirma itinerario</h3>
                </div>
                <div class="icon-box group-hover/item:translate-x-2 transition-transform duration-500"><i class="fas fa-file-contract"></i></div>
                <h4 class="text-lg font-bold text-[#c66b6b] mb-2">Firma la promesa de compraventa</h4>
                <p class="text-gray-600 text-sm leading-relaxed">
                    Tu plan de vuelo queda oficializado. Antes de firmar, tendrás un espacio exclusivo de atención con nuestros asesores comerciales para resolver cualquier duda técnica como parte de tu check-in pre-vuelo. Recibirás todo cómodamente en tu email.
                </p>
            </div>
            <div class="hidden md:block w-2/12 text-center text-4xl text-[#c66b6b]/20 group-hover/item:text-[#c66b6b] transition-colors duration-500">
                <i class="fas fa-signature"></i>
            </div>
            <div class="w-full md:w-5/12 mt-6 md:mt-0 ml-12 md:ml-0 image-window h-64 bg-gray-200">
                <img src="https://maz.com.co/wp-content/uploads/2025/09/atardecer-afflora-apartamento-barranquilla-maz-constructora-sep2025-min-optimized.jpg" alt="Proyecto Afflora Maz" class="w-full h-full object-cover transform group-hover/item:scale-110 transition-transform duration-700 filter brightness-95">
                <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent flex items-end p-6">
                    <p class="text-white font-bold tracking-widest text-xs uppercase">Proyecto Afflora - Barranquilla</p>
                </div>
            </div>
        </div>

        <div class="flex flex-col md:flex-row items-center justify-between mb-36 relative z-10 group/item">
            <div class="w-full md:w-5/12 step-card p-8 ml-12 md:ml-0">
                <div class="flex items-center gap-4 mb-6">
                    <div class="step-number">05</div>
                    <h3 class="text-lg md:text-xl font-bold text-gray-800 uppercase tracking-tighter">Listos para despegue</h3>
                </div>
                <div class="icon-box group-hover/item:translate-y-[-5px] transition-transform duration-500"><i class="fas fa-plane-takeoff"></i></div>
                <h4 class="text-lg font-bold text-[#c66b6b] mb-2">Firma de escritura</h4>
                <p class="text-gray-600 text-sm leading-relaxed mb-4">
                    Estamos en la pista listos para despegar hacia la meta. Te notificaremos con la antelación debida el día, la notaría presencial y los requisitos exactos.
                </p>
                <div class="bg-red-50/80 p-3 rounded-xl border-l-4 border-[#c66b6b] text-[11px] text-gray-600 font-medium italic">
                    <i class="fas fa-info-circle mr-1 text-[#c66b6b]"></i> Revisa con cuidado la minuta de escrituración que llegará adjunta a tu correo electrónico.
                </div>
            </div>
            <div class="hidden md:block w-2/12 text-center text-4xl text-[#c66b6b]/20 group-hover/item:text-[#c66b6b] transition-colors duration-500">
                <i class="fas fa-pen-fancy"></i>
            </div>
            <div class="w-full md:w-5/12 mt-6 md:mt-0 ml-12 md:ml-0 image-window h-64 bg-gray-200">
                <img src="https://maz.com.co/wp-content/uploads/2025/10/render-3-apartamentos-inversion-bogota-centro-maz-constructora-oct2025-min-optimized.jpg" alt="Inversion Bogota Maz" class="w-full h-full object-cover transform group-hover/item:scale-110 transition-transform duration-700 filter brightness-95">
                <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent flex items-end p-6">
                    <p class="text-white font-bold tracking-widest text-xs uppercase">Bogotá Centro - Inversión</p>
                </div>
            </div>
        </div>

        <div class="flex flex-col md:flex-row-reverse items-center justify-between mb-36 relative z-10 group/item">
            <div class="w-full md:w-5/12 step-card p-8 ml-12 md:ml-0 border-2 border-[#c66b6b] bg-white">
                <div class="flex items-center gap-4 mb-6">
                    <div class="step-number">06</div>
                    <h3 class="text-lg md:text-xl font-bold text-gray-800 uppercase tracking-tighter">¡Llegamos a destino!</h3>
                </div>
                <div class="icon-box bg-[#c66b6b] text-white shadow-lg shadow-red-200"><i class="fas fa-key"></i></div>
                <h4 class="text-lg font-bold text-[#c66b6b] mb-2">Entrega de tu inmueble</h4>
                <p class="text-gray-600 text-sm leading-relaxed">
                    ¡El momento cumbre del viaje ha llegado! Te entregamos oficialmente las llaves de tu nuevo hogar junto con planos arquitectónicos finales, el manual completo de propietario y recomendaciones clave de uso.
                </p>
            </div>
            <div class="hidden md:block w-2/12 text-center text-4xl text-[#c66b6b]/20 group-hover/item:text-[#c66b6b] transition-colors duration-500">
                <i class="fas fa-home"></i>
            </div>
            <div class="w-full md:w-5/12 mt-6 md:mt-0 ml-12 md:ml-0 image-window h-64 bg-gray-200">
                <img src="https://maz.com.co/wp-content/uploads/2025/07/piscina-marama-apartamentos-vis-villavicencio-maz-constructora-jul-2025-optimized.jpg" alt="Entrega Apartamento Maz" class="w-full h-full object-cover transform group-hover/item:scale-110 transition-transform duration-700 filter brightness-95">
                <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent flex items-end p-6">
                    <p class="text-white font-bold tracking-widest text-xs uppercase">Proyecto Maramá - Villavicencio</p>
                </div>
            </div>
        </div>

        <div class="flex flex-col md:flex-row items-center justify-between mb-20 relative z-10 group/item">
            <div class="w-full md:w-5/12 step-card p-8 ml-12 md:ml-0">
                <div class="flex items-center gap-4 mb-6">
                    <div class="step-number">07</div>
                    <h3 class="text-lg md:text-xl font-bold text-gray-800 uppercase tracking-tighter">Seguimos en ruta</h3>
                </div>
                <div class="icon-box group-hover/item:scale-95 transition-transform duration-500"><i class="fas fa-hand-holding-heart"></i></div>
                <h4 class="text-lg font-bold text-[#c66b6b] mb-2">Garantías post-viaje</h4>
                <p class="text-gray-600 text-sm leading-relaxed">
                    Aunque hayas aterrizado con éxito, seguimos volando contigo. Tu tranquilidad está protegida con coberturas robustas: reporte de novedades en acabados durante el primer año y hasta 10 años de respaldo en elementos estructurales.
                </p>
            </div>
            <div class="hidden md:block w-2/12 text-center text-4xl text-[#c66b6b]/20 group-hover/item:text-[#c66b6b] transition-colors duration-500">
                <i class="fas fa-user-shield"></i>
            </div>
            <div class="w-full md:w-5/12 mt-6 md:mt-0 ml-12 md:ml-0 image-window h-64 bg-gray-200">
                <img src="https://maz.com.co/wp-content/uploads/2024/09/more-maz-fachada-vista2-apartamento-tocancipa-min-optimized.jpg" alt="Garantias Maz" class="w-full h-full object-cover transform group-hover/item:scale-110 transition-transform duration-700 filter brightness-95">
                <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent flex items-end p-6">
                    <p class="text-white font-bold tracking-widest text-xs uppercase">Respaldo y Solidez Nacional</p>
                </div>
            </div>
        </div>
    </section>

    <footer class="bg-white pt-20 pb-10 px-6 text-center border-t border-gray-100">
        <div class="max-w-4xl mx-auto">
            <img src="https://maz.com.co/wp-content/uploads/2022/07/maz-logo.svg" alt="Maz Logo" class="h-16 mx-auto mb-8 maz-logo">
            <h2 class="text-3xl font-light mb-6 text-gray-800">¿Listo para tu <span class="font-bold text-[#c66b6b] font-serif-maz">próximo despegue?</span></h2>
            <p class="text-gray-500 mb-10 italic">"Nuestra tripulación corporativa está lista para acompañarte en cada kilómetro del trayecto hacia tu nuevo espacio."</p>
            
            <div class="flex justify-center gap-6 mb-12">
                <a href="#" class="w-12 h-12 rounded-full bg-gray-100 flex items-center justify-center hover:bg-[#c66b6b] hover:text-white text-gray-600 transition-all shadow-sm"><i class="fab fa-whatsapp"></i></a>
                <a href="#" class="w-12 h-12 rounded-full bg-gray-100 flex items-center justify-center hover:bg-[#c66b6b] hover:text-white text-gray-600 transition-all shadow-sm"><i class="fab fa-instagram"></i></a>
                <a href="#" class="w-12 h-12 rounded-full bg-gray-100 flex items-center justify-center hover:bg-[#c66b6b] hover:text-white text-gray-600 transition-all shadow-sm"><i class="fab fa-facebook-f"></i></a>
            </div>
            
            <div class="text-[10px] text-gray-400 uppercase tracking-widest border-t pt-8">
                MAZ CONSTRUCTORA © 2026 - TU HOGAR, NUESTRO DESTINO
            </div>
        </div>
    </footer>

    <script>
        // Función interactiva para abrir/cerrar el Check-in
        function toggleModal() {
            const modal = document.getElementById('checkinModal');
            modal.classList.toggle('hidden');
        }

        // Animación mejorada del avión e iluminación de la estela siguiendo el scroll
        const plane = document.getElementById('plane');
        const pathFill = document.getElementById('pathFill');
        const timelineSection = document.getElementById('viaje');
        
        window.addEventListener('scroll', () => {
            const timelineTop = timelineSection.offsetTop;
            const timelineHeight = timelineSection.offsetHeight;
            const scrollY = window.scrollY;
            
            let planePos = (scrollY - timelineTop + 240); 
            
            if(planePos < 0) planePos = 0;
            if(planePos > timelineHeight - 40) planePos = timelineHeight - 40;
            
            plane.style.top = planePos + 'px';
            pathFill.style.height = planePos + 'px';
        });

        // Revelación fluida mediante IntersectionObserver
        const observerOptions = {
            threshold: 0.1,
            rootMargin: "0px 0px -50px 0px"
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0) scale(1)';
                }
            });
        }, observerOptions);

        document.querySelectorAll('.step-card, .image-window').forEach(el => {
            el.style.opacity = '0';
            el.style.transform = 'translateY(40px) scale(0.96)';
            el.style.transition = 'all 0.9s cubic-bezier(0.165, 0.84, 0.44, 1)';
            observer.observe(el);
        });
    </script>
</body>
</html>
