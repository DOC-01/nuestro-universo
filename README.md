<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nuestro Universo - Daniela</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            overflow: hidden;
            background-color: #030008;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            color: white;
            perspective: 1000px;
            width: 100vw;
            height: 100vh;
            user-select: none;
        }

        #bg-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        #universo-3d {
            position: absolute;
            width: 100%;
            height: 100%;
            z-index: 10;
            cursor: grab;
            transform-style: preserve-3d;
            display: block; /* Mantenemos el bloque activo */
            opacity: 0;
            visibility: hidden;
            transform: rotateX(0deg) rotateY(0deg);
            transition: opacity 0.6s ease, visibility 0.6s ease;
        }

        #universo-3d:active {
            cursor: grabbing;
        }

        .card-bienvenida {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 100;
            background: rgba(10, 4, 18, 0.92);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 71, 108, 0.3);
            padding: 45px 35px;
            border-radius: 28px;
            text-align: center;
            width: 90%;
            max-width: 480px;
            box-shadow: 0 30px 70px rgba(0,0,0,0.9);
            transition: opacity 0.4s ease;
        }

        .title {
            font-size: 1.6rem;
            letter-spacing: 4px;
            margin-bottom: 22px;
            color: #ff477e;
            text-transform: uppercase;
            font-weight: bold;
        }

        .message {
            font-size: 1.05rem;
            line-height: 1.8;
            margin-bottom: 35px;
            white-space: pre-line;
            color: #e2e4eb;
        }

        .btn-explorar {
            background: linear-gradient(45deg, #ff4757, #ff477e);
            color: white;
            border: none;
            padding: 16px 45px;
            border-radius: 35px;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 6px 25px rgba(255, 71, 87, 0.35);
        }

        .btn-explorar:hover {
            transform: scale(1.03);
            box-shadow: 0 10px 35px rgba(255, 71, 126, 0.65);
        }

        .planeta {
            position: absolute;
            width: 110px;
            height: 110px;
            transform-style: preserve-3d;
            cursor: pointer;
            z-index: 50;
        }

        .esfera-media {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            position: relative;
            box-shadow: inset -15px -15px 25px rgba(0,0,0,0.85), 0 0 15px rgba(255, 71, 126, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.15);
            overflow: hidden;
            background: #0d0416;
            transition: transform 0.3s ease, border-color 0.3s ease;
        }

        .esfera-media img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            pointer-events: none;
        }

        .planeta:hover .esfera-media {
            box-shadow: inset -10px -10px 20px rgba(0,0,0,0.7), 0 0 35px #ff477e;
            border-color: #ff477e;
            transform: scale(1.1);
        }

        .mensaje-flotante {
            position: absolute;
            top: -50px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(12, 5, 22, 0.95);
            border: 1px solid rgba(255, 71, 126, 0.4);
            padding: 6px 12px;
            border-radius: 8px;
            font-size: 0.8rem;
            text-align: center;
            width: 180px;
            pointer-events: none;
            box-shadow: 0 8px 25px rgba(0,0,0,0.6);
            opacity: 0;
            transition: opacity 0.2s ease;
            z-index: 60;
        }

        .planeta:hover .mensaje-flotante {
            opacity: 1;
        }

        #visor-recuerdo {
            position: absolute;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(4, 2, 8, 0.94);
            backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px);
            z-index: 200;
            display: none;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transition: opacity 0.4s ease;
        }

        .visor-contenido {
            background: rgba(20, 10, 32, 0.85);
            border: 1px solid rgba(255, 71, 126, 0.35);
            border-radius: 24px;
            padding: 25px;
            max-width: 500px;
            width: 90%;
            text-align: center;
            box-shadow: 0 40px 90px rgba(0,0,0,0.95);
            transform: scale(0.9);
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.15);
        }

        .visor-img {
            width: 100%;
            max-height: 55vh;
            object-fit: contain;
            border-radius: 16px;
            margin-bottom: 20px;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .visor-texto {
            font-size: 1.1rem;
            line-height: 1.7;
            color: #f1f3f9;
            font-style: italic;
        }

        .btn-cerrar {
            position: absolute;
            top: 25px;
            right: 30px;
            background: none;
            border: none;
            color: rgba(255,255,255,0.6);
            font-size: 2.8rem;
            cursor: pointer;
            z-index: 220;
        }
        .btn-cerrar:hover { color: #ff477e; }

        #audio-controles {
            position: absolute;
            top: 25px;
            right: 30px;
            z-index: 150;
            display: none;
            background: rgba(12, 5, 22, 0.8);
            border: 1px solid rgba(255, 71, 126, 0.2);
            padding: 8px 18px;
            border-radius: 20px;
            backdrop-filter: blur(10px);
        }
        .btn-audio {
            background: none;
            border: none;
            color: white;
            font-size: 0.85rem;
            cursor: pointer;
            font-weight: bold;
            letter-spacing: 1px;
        }

        #indicador {
            position: absolute;
            top: 25px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(5, 1, 10, 0.85);
            padding: 12px 30px;
            border-radius: 30px;
            font-size: 0.85rem;
            border: 1px solid rgba(255, 71, 126, 0.2);
            z-index: 10;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.5s ease;
        }
    </style>
</head>
<body>

    <canvas id="bg-canvas"></canvas>
    
    <audio id="musica-fondo" loop preload="auto">
        <source src="cancion.mp3" type="audio/mpeg">
    </audio>
    
    <div id="audio-controles">
        <button class="btn-audio" onclick="toggleAudio()" id="btn-audio-txt">🎵 REPRODUCIR MÚSICA</button>
    </div>

    <div id="indicador">Haz clic en un recuerdo para ampliarlo. Arrastra para explorar el espacio.</div>

    <div class="card-bienvenida" id="start-screen">
        <div style="font-size: 42px; margin-bottom: 12px;">❤️</div>
        <div class="title">Para: Daniela</div>
        <div class="message">Fuiste todo mi universo.

Tengo que seguir adelante; extrañarte en esta soledad no me gusta... no es el tipo de cosas que suelo decir ni sé expresarme bien, pero quédate con esto:

Vive, que yo viviré por ti. Te amo.</div>
        <button class="btn-explorar" onclick="entrarAlUniverso()">Recordar nuestro universo</button>
    </div>

    <div id="universo-3d">
        <div class="planeta" style="transform: translate3d(-350px, -120px, -100px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.10.59 PM (1).jpeg" data-txt='"Cada risa y cada instante juntos se quedan grabados aquí."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(320px, 150px, -200px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.10.59 PM.jpeg" data-txt='"Tu fe en nosotros es algo que atesoro profundamente."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(-100px, 280px, -400px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.00 PM (1).jpeg" data-txt='"Los mejores recuerdos no se borran, se transforman."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(150px, -250px, -150px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.00 PM (2).jpeg" data-txt='"Te deseo lo mejor en cada paso de tu propio camino."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(-400px, 180px, -300px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.00 PM (3).jpeg" data-txt='"Incluso en la distancia, el cariño permanece intacto."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(450px, -100px, -350px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.00 PM (4).jpeg" data-txt='"Compartiendo momentos únicos e inolvidables."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(-200px, -320px, -450px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.00 PM.jpeg" data-txt='"Tu sonrisa siempre iluminó mis días más oscuros."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(280px, -300px, -500px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.03 PM (1).jpeg" data-txt='"La complicidad que tuvimos es algo único."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(-50px, -50px, -600px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.03 PM (2).jpeg" data-txt='"Pequeños detalles que guardo con un cariño inmenso."'></div>
        </div>
        <div class="planeta" style="transform: translate3d(0px, 200px, -150px);" onclick="abrirRecuerdo(this)">
            <div class="mensaje-flotante">Ver recuerdo</div>
            <div class="esfera-media"><img src="WhatsApp Image 2026-06-18 at 8.11.0
