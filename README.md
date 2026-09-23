
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Gafur Demo | Official Fanpage</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Montserrat', sans-serif;
            background: linear-gradient(-45deg, #09090e, #180a2b, #0f0c29, #16102b);
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite;
            color: #ffffff;
            display: flex;
            justify-content: center;
            min-height: 100vh;
            padding: 50px 20px;
        }

        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .container {
            width: 100%;
            max-width: 420px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .brand-title {
            font-size: 32px;
            font-weight: 800;
            text-align: center;
            color: #ffffff;
            letter-spacing: 2px;
            margin-bottom: 8px;
            text-shadow: 0 4px 15px rgba(0,0,0,0.6);
        }

        .main-desc {
            font-size: 11px;
            font-weight: 500;
            text-align: center;
            margin-bottom: 40px;
            color: #a0a0b5;
            text-transform: uppercase;
            letter-spacing: 2px;
            line-height: 1.5;
        }

        .section-title {
            margin: 25px 0 15px;
            font-size: 11px;
            font-weight: 600;
            color: #8b8b99;
            text-transform: uppercase;
            letter-spacing: 2px;
            text-align: center;
            width: 100%;
        }

        .links-wrapper {
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .btn {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            width: 100%;
            padding: 20px 25px;
            border-radius: 50px;
            text-decoration: none;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
            position: relative;
        }

        .btn i {
            font-size: 20px;
        }

        .btn-glass {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #ffffff;
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
        }

        .btn-glass:hover {
            background: rgba(255, 255, 255, 0.1);
            border-color: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
        }

        .btn-premium {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            border: none;
            color: #ffffff;
            box-shadow: 0 10px 25px rgba(37, 117, 252, 0.4);
        }

        .btn-premium:hover {
            box-shadow: 0 15px 35px rgba(37, 117, 252, 0.6);
            transform: translateY(-2px);
            filter: brightness(1.1);
        }

        .btn-release {
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.15);
            color: #ffffff;
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
        }
        
        .btn-release:hover {
            background: rgba(255, 255, 255, 0.15);
            border-color: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
        }

        .video-container {
            margin-top: 35px;
            width: 100%;
            border-radius: 25px;
            overflow: hidden;
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.5);
            padding: 5px;
            backdrop-filter: blur(10px);
        }

        .video-container iframe {
            width: 100%;
            aspect-ratio: 16/9;
            border: none;
            display: block;
            border-radius: 20px;
        }
    </style>
</head>
<body>

    <div class="container">
        
        <!-- Заголовок и описание -->
        <h1 class="brand-title">GAFUR DEMO</h1>
        <p class="main-desc">Самый крупный и официальный<br>фан аккаунт</p>

        <div class="links-wrapper">
            <!-- Соцсети с иконками -->
            <a href="https://instagram.com/gafur.demo" target="_blank" class="btn btn-glass">
                <i class="fa-brands fa-instagram"></i> Instagram
            </a>
            
            <a href="https://youtube.com/@gafurdemo" target="_blank" class="btn btn-glass">
                <i class="fa-brands fa-youtube"></i> YouTube
            </a>
            
            <a href="https://tiktok.com/@gafur.demo" target="_blank" class="btn btn-glass">
                <i class="fa-brands fa-tiktok"></i> TikTok
            </a>
            
            <!-- Официальный аккаунт -->
            <div class="section-title">Официальный аккаунт</div>
            <a href="https://instagram.com/gafur.lv" target="_blank" class="btn btn-premium">
                <i class="fa-brands fa-instagram"></i> @gafur.lv <i class="fa-solid fa-circle-check" style="font-size: 16px;"></i>
            </a>

            <!-- Релиз -->
            <div class="section-title">Новый релиз ARMON</div>
            <a href="https://band.link/gafur_armon" target="_blank" class="btn btn-release">
                <i class="fa-solid fa-music"></i> Слушать на всех площадках
            </a>
        </div>

        <!-- YouTube Плеер -->
        <div class="video-container">
            <iframe src="https://www.youtube.com/embed/c3hGzK-gwBk" title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        </div>

    </div>

</body>
</html>
