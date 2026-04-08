<!DOCTYPE html>
<html lang="zh-CN" data-theme="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>科技用之于民 | Tech for the People</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Space+Grotesk:wght@300;500;700&display=swap" rel="stylesheet">
    <style>
        /* 基础变量 - 夜间模式（默认） */
        :root {
            --bg-primary: #0a0a0a;
            --bg-secondary: #111;
            --text-primary: #ffffff;
            --text-secondary: rgba(255, 255, 255, 0.6);
            --border-color: rgba(255, 255, 255, 0.1);
            --accent-glow: rgba(120, 119, 198, 0.15);
            --star-opacity: 1;
            --day-opacity: 0;
            --github-filter: invert(1);
        }
        
        /* 白天模式变量 - 通过属性选择器覆盖 */
        [data-theme="light"] {
            --bg-primary: #f5f5f0;
            --bg-secondary: #ffffff;
            --text-primary: #1a1a1a;
            --text-secondary: rgba(0, 0, 0, 0.6);
            --border-color: rgba(0, 0, 0, 0.1);
            --accent-glow: rgba(255, 193, 7, 0.15);
            --star-opacity: 0;
            --day-opacity: 1;
            --github-filter: none;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        html {
            transition: background-color 0.8s ease;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-primary);
            color: var(--text-primary);
            overflow-x: hidden;
            cursor: none;
            transition: background-color 0.8s ease, color 0.8s ease;
            min-height: 100vh;
        }
        
        .font-display {
            font-family: 'Space Grotesk', sans-serif;
        }
        
        /* Custom Cursor */
        .cursor {
            width: 20px;
            height: 20px;
            border: 1px solid var(--text-primary);
            border-radius: 50%;
            position: fixed;
            pointer-events: none;
            z-index: 9999;
            transition: width 0.3s, height 0.3s, background 0.3s, border-color 0.8s;
            mix-blend-mode: difference;
        }
        
        .cursor.hover {
            width: 60px;
            height: 60px;
            background: rgba(128, 128, 128, 0.1);
        }
        
        /* Grain Overlay */
        .grain {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 9998;
            opacity: 0.03;
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
            transition: opacity 0.8s;
        }
        
        [data-theme="light"] .grain {
            opacity: 0.02;
        }
        
        /* Stars Container */
        .stars-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
            opacity: var(--star-opacity);
            transition: opacity 0.8s ease;
        }
        
        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite ease-in-out;
            box-shadow: 0 0 4px white;
        }
        
        @keyframes twinkle {
            0%, 100% { opacity: 0.2; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.5); }
        }
        
        /* Day Elements Container */
        .day-elements {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
            opacity: var(--day-opacity);
            transition: opacity 0.8s ease;
        }
        
        .sun {
            position: absolute;
            top: 10%;
            right: 10%;
            width: 80px;
            height: 80px;
            background: radial-gradient(circle, #ffd700 0%, #ffa500 70%, transparent 100%);
            border-radius: 50%;
            box-shadow: 0 0 60px rgba(255, 215, 0, 0.5), 0 0 100px rgba(255, 165, 0, 0.3);
            animation: sunPulse 4s ease-in-out infinite;
        }
        
        @keyframes sunPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }
        
        .cloud {
            position: absolute;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 100px;
            animation: float 20s infinite ease-in-out;
        }
        
        .cloud::before,
        .cloud::after {
            content: '';
            position: absolute;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 100px;
        }
        
        .cloud:nth-child(2) {
            width: 100px;
            height: 40px;
            top: 20%;
            left: 10%;
        }
        
        .cloud:nth-child(2)::before {
            width: 50px;
            height: 50px;
            top: -25px;
            left: 10px;
        }
        
        .cloud:nth-child(2)::after {
            width: 60px;
            height: 40px;
            top: -15px;
            right: 10px;
        }
        
        .cloud:nth-child(3) {
            width: 80px;
            height: 35px;
            top: 40%;
            right: 20%;
            animation-delay: -5s;
        }
        
        .cloud:nth-child(3)::before {
            width: 40px;
            height: 40px;
            top: -20px;
            left: 15px;
        }
        
        .cloud:nth-child(3)::after {
            width: 50px;
            height: 35px;
            top: -10px;
            right: 15px;
        }
        
        .cloud:nth-child(4) {
            width: 120px;
            height: 45px;
            top: 60%;
            left: 30%;
            animation-delay: -10s;
        }
        
        .cloud:nth-child(4)::before {
            width: 60px;
            height: 60px;
            top: -30px;
            left: 20px;
        }
        
        .cloud:nth-child(4)::after {
            width: 70px;
            height: 45px;
            top: -20px;
            right: 20px;
        }
        
        /* Theme Toggle Button */
        .theme-toggle {
            position: fixed;
            top: 2rem;
            right: 6rem;
            z-index: 1000;
            width: 60px;
            height: 32px;
            background: var(--border-color);
            border-radius: 16px;
            cursor: pointer;
            padding: 4px;
            transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1), background 0.8s;
            border: 1px solid var(--border-color);
        }
        
        .theme-toggle:hover {
            transform: scale(1.1);
        }
        
        .toggle-slider {
            width: 24px;
            height: 24px;
            background: var(--text-primary);
            border-radius: 50%;
            position: relative;
            transition: transform 0.5s cubic-bezier(0.34, 1.56, 0.64, 1), background 0.8s;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
        }
        
        [data-theme="light"] .toggle-slider {
            transform: translateX(28px);
        }
        
        .toggle-icon {
            position: absolute;
            transition: opacity 0.3s;
        }
        
        .moon-icon {
            opacity: 1;
        }
        
        .sun-icon {
            opacity: 0;
        }
        
        [data-theme="light"] .moon-icon {
            opacity: 0;
        }
        
        [data-theme="light"] .sun-icon {
            opacity: 1;
        }
        
        /* Hero Section */
        .hero {
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
            overflow: hidden;
        }
        
        .hero-bg {
            position: absolute;
            width: 150%;
            height: 150%;
            background: radial-gradient(circle at 50% 50%, var(--accent-glow) 0%, transparent 50%);
            animation: pulse 8s ease-in-out infinite;
            transition: background 0.8s;
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); opacity: 0.5; }
            50% { transform: scale(1.1); opacity: 0.8; }
        }
        
        /* Animated Characters Container */
        .characters-container {
            position: relative;
            width: 400px;
            height: 300px;
            display: flex;
            align-items: flex-end;
            justify-content: center;
            gap: 0;
            z-index: 10;
            cursor: pointer;
        }
        
        /* Individual Character Styles */
        .character {
            position: relative;
            transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
            will-change: transform;
        }
        
        /* Orange Semi-circle (Front) */
        .char-orange {
            width: 120px;
            height: 60px;
            background: #FF6B2C;
            border-radius: 60px 60px 0 0;
            z-index: 4;
            transform-origin: bottom center;
            animation: breathe 3s ease-in-out infinite;
        }
        
        .char-orange .face {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 15px;
            align-items: center;
        }
        
        .char-orange .eye {
            width: 8px;
            height: 8px;
            background: #000;
            border-radius: 50%;
            position: relative;
            overflow: hidden;
        }
        
        .char-orange .pupil {
            position: absolute;
            width: 4px;
            height: 4px;
            background: #fff;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            transition: transform 0.1s ease-out;
        }
        
        .char-orange .mouth {
            position: absolute;
            width: 16px;
            height: 8px;
            border-bottom: 3px solid #000;
            border-radius: 0 0 16px 16px;
            top: 5px;
            left: 50%;
            transform: translateX(-50%);
        }
        
        /* Blue Rectangle (Tall) */
        .char-blue {
            width: 80px;
            height: 140px;
            background: #4C24F5;
            border-radius: 4px;
            z-index: 3;
            margin-left: -10px;
            margin-bottom: 0;
            transform-origin: bottom center;
            animation: breathe 3s ease-in-out infinite 0.2s;
            position: relative;
        }
        
        .char-blue .face {
            position: absolute;
            top: 30px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 20px;
        }
        
        .char-blue .eye {
            width: 10px;
            height: 10px;
            background: #fff;
            border-radius: 50%;
            position: relative;
            overflow: hidden;
        }
        
        .char-blue .pupil {
            position: absolute;
            width: 5px;
            height: 5px;
            background: #000;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            transition: transform 0.1s ease-out;
        }
        
        .char-blue .mouth {
            position: absolute;
            width: 20px;
            height: 3px;
            background: #000;
            border-radius: 2px;
            top: 50px;
            left: 50%;
            transform: translateX(-50%);
        }
        
        /* Black Rectangle */
        .char-black {
            width: 50px;
            height: 90px;
            background: #0a0a0a;
            border-radius: 4px;
            z-index: 2;
            margin-left: -5px;
            transform-origin: bottom center;
            animation: breathe 3s ease-in-out infinite 0.4s;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        [data-theme="light"] .char-black {
            border: 1px solid rgba(0,0,0,0.1);
        }
        
        .char-black .face {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 8px;
        }
        
        .char-black .eye {
            width: 8px;
            height: 8px;
            background: #fff;
            border-radius: 50%;
            position: relative;
            overflow: hidden;
        }
        
        .char-black .pupil {
            position: absolute;
            width: 3px;
            height: 3px;
            background: #000;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            transition: transform 0.1s ease-out;
        }
        
        /* Yellow Semi-circle */
        .char-yellow {
            width: 60px;
            height: 70px;
            background: #FFD700;
            border-radius: 30px 30px 0 0;
            z-index: 1;
            margin-left: -5px;
            transform-origin: bottom center;
            animation: breathe 3s ease-in-out infinite 0.6s;
        }
        
        .char-yellow .face {
            position: absolute;
            top: 25px;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
        }
        
        .char-yellow .eye {
            width: 6px;
            height: 6px;
            background: #000;
            border-radius: 50%;
            position: absolute;
            top: 0;
            overflow: hidden;
        }
        
        .char-yellow .eye:first-child {
            left: 12px;
        }
        
        .char-yellow .eye:last-child {
            right: 12px;
        }
        
        .char-yellow .pupil {
            position: absolute;
            width: 2px;
            height: 2px;
            background: #fff;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            transition: transform 0.1s ease-out;
        }
        
        .char-yellow .beak {
            position: absolute;
            width: 20px;
            height: 4px;
            background: #000;
            top: 8px;
            left: 50%;
            transform: translateX(-50%);
            border-radius: 2px;
        }
        
        @keyframes breathe {
            0%, 100% { transform: scaleY(1) scaleX(1); }
            50% { transform: scaleY(1.02) scaleX(0.98); }
        }
        
        /* Squeeze animation on click/hover */
        .character.squeezed {
            animation: squeeze 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        
        @keyframes squeeze {
            0% { transform: scaleY(1) scaleX(1); }
            30% { transform: scaleY(0.8) scaleX(1.2); }
            60% { transform: scaleY(1.1) scaleX(0.9); }
            100% { transform: scaleY(1) scaleX(1); }
        }
        
        /* Navigation */
        .nav {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            padding: 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 100;
            mix-blend-mode: difference;
        }
        
        .logo {
            font-family: 'Space Grotesk', sans-serif;
            font-weight: 700;
            font-size: 1.2rem;
            letter-spacing: -0.02em;
            max-width: 300px;
            line-height: 1.4;
        }
        
        .menu-trigger {
            width: 40px;
            height: 40px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 6px;
            cursor: pointer;
            transition: transform 0.3s;
        }
        
        .menu-trigger:hover {
            transform: scale(1.1);
        }
        
        .menu-trigger span {
            width: 24px;
            height: 1px;
            background: var(--text-primary);
            transition: all 0.3s;
        }
        
        .menu-trigger.active span:nth-child(1) {
            transform: rotate(45deg) translate(5px, 5px);
        }
        
        .menu-trigger.active span:nth-child(2) {
            opacity: 0;
        }
        
        .menu-trigger.active span:nth-child(3) {
            transform: rotate(-45deg) translate(5px, -5px);
        }
        
        /* Full Screen Menu */
        .fullscreen-menu {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--bg-primary);
            z-index: 99;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.5s ease, background 0.8s;
        }
        
        .fullscreen-menu.active {
            opacity: 1;
            pointer-events: all;
        }
        
        .menu-item {
            font-family: 'Space Grotesk', sans-serif;
            font-size: clamp(2rem, 6vw, 4rem);
            font-weight: 300;
            margin: 1rem 0;
            opacity: 0;
            transform: translateY(50px);
            transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1), color 0.8s;
            cursor: pointer;
            position: relative;
            text-decoration: none;
            color: var(--text-primary);
        }
        
        .fullscreen-menu.active .menu-item {
            opacity: 1;
            transform: translateY(0);
        }
        
        .fullscreen-menu.active .menu-item:nth-child(1) { transition-delay: 0.1s; }
        .fullscreen-menu.active .menu-item:nth-child(2) { transition-delay: 0.2s; }
        .fullscreen-menu.active .menu-item:nth-child(3) { transition-delay: 0.3s; }
        
        .menu-item::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--text-primary);
            transition: width 0.3s ease;
        }
        
        .menu-item:hover::after {
            width: 100%;
        }
        
        /* Content Sections */
        .section {
            min-height: 100vh;
            padding: 10rem 2rem;
            position: relative;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .content-wrapper {
            max-width: 1200px;
            width: 100%;
            margin: 0 auto;
        }
        
        .section-title {
            font-size: clamp(2rem, 5vw, 4rem);
            font-weight: 300;
            margin-bottom: 3rem;
            position: relative;
            display: inline-block;
            color: var(--text-primary);
            transition: color 0.8s;
        }
        
        .section-title::before {
            content: '';
            position: absolute;
            left: -2rem;
            top: 50%;
            width: 1rem;
            height: 1px;
            background: var(--text-secondary);
            transition: background 0.8s;
        }
        
        /* Interactive Text */
        .interactive-text {
            font-size: 1.5rem;
            line-height: 2;
            color: var(--text-secondary);
            max-width: 800px;
            transition: color 0.8s;
        }
        
        .interactive-text .word {
            display: inline-block;
            transition: all 0.3s;
            cursor: default;
        }
        
        .interactive-text .word:hover {
            color: var(--text-primary);
            transform: scale(1.1);
            text-shadow: 0 0 20px var(--accent-glow);
        }
        
        /* Links Section */
        .links-section {
            background: linear-gradient(to bottom, var(--bg-primary), var(--bg-secondary));
            transition: background 0.8s;
        }
        
        .links-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            margin-top: 4rem;
        }
        
        .link-card {
            border: 1px solid var(--border-color);
            padding: 3rem;
            position: relative;
            overflow: hidden;
            transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1), border-color 0.8s, background 0.8s;
            text-decoration: none;
            color: var(--text-primary);
            display: block;
            background: var(--bg-primary);
        }
        
        .link-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at var(--x, 50%) var(--y, 50%), var(--accent-glow) 0%, transparent 60%);
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .link-card:hover::before {
            opacity: 1;
        }
        
        .link-card:hover {
            border-color: var(--text-secondary);
            transform: translateY(-5px);
        }
        
        .link-icon {
            width: 60px;
            height: 60px;
            margin-bottom: 1.5rem;
            opacity: 0.8;
            transition: opacity 0.3s, transform 0.3s, filter 0.8s;
            object-fit: contain;
            filter: var(--github-filter);
        }
        
        /* B站图标在白天模式下不需要反色，夜间也不需要，因为它是粉色的 */
        .link-icon[src*="bilibili"] {
            filter: none !important;
        }
        
        .link-card:hover .link-icon {
            opacity: 1;
            transform: scale(1.1);
        }
        
        .link-title {
            font-family: 'Space Grotesk', sans-serif;
            font-size: 1.5rem;
            font-weight: 500;
            margin-bottom: 0.5rem;
        }
        
        .link-desc {
            font-size: 0.9rem;
            color: var(--text-secondary);
            transition: color 0.8s;
        }
        
        .link-arrow {
            position: absolute;
            right: 2rem;
            top: 50%;
            transform: translateY(-50%) translateX(-10px);
            opacity: 0;
            transition: all 0.3s;
            color: var(--text-primary);
        }
        
        .link-card:hover .link-arrow {
            opacity: 1;
            transform: translateY(-50%) translateX(0);
        }
        
        /* Animated Border */
        .animated-border {
            position: absolute;
            inset: 0;
            border: 1px solid transparent;
            background: linear-gradient(90deg, transparent, var(--text-secondary), transparent) border-box;
            -webkit-mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
            mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
            -webkit-mask-composite: xor;
            mask-composite: exclude;
            opacity: 0;
            transition: opacity 0.3s;
            background-size: 200% 100%;
        }
        
        .link-card:hover .animated-border {
            opacity: 1;
            animation: borderRotate 3s linear infinite;
        }
        
        @keyframes borderRotate {
            0% { background-position: 0% 50%; }
            100% { background-position: 200% 50%; }
        }
        
        /* Footer */
        .footer {
            padding: 4rem 2rem;
            text-align: center;
            border-top: 1px solid var(--border-color);
            transition: border-color 0.8s;
        }
        
        .footer-text {
            font-size: 0.9rem;
            color: var(--text-secondary);
            letter-spacing: 0.2em;
            transition: color 0.8s;
        }
        
        /* Scroll Indicator */
        .scroll-indicator {
            position: absolute;
            bottom: 3rem;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 1rem;
            opacity: 0;
            animation: fadeIn 1s ease 1.5s forwards;
        }
        
        .scroll-text {
            font-size: 0.7rem;
            letter-spacing: 0.3em;
            text-transform: uppercase;
            color: var(--text-secondary);
            transition: color 0.8s;
        }
        
        .scroll-line {
            width: 1px;
            height: 60px;
            background: linear-gradient(to bottom, var(--text-secondary), transparent);
            animation: scrollPulse 2s infinite;
            transition: background 0.8s;
        }
        
        @keyframes fadeIn {
            to { opacity: 1; }
        }
        
        @keyframes scrollPulse {
            0%, 100% { transform: scaleY(1); opacity: 1; }
            50% { transform: scaleY(0.5); opacity: 0.5; }
        }
        
        /* Responsive */
        @media (max-width: 768px) {
            body {
                cursor: auto;
            }
            .cursor {
                display: none;
            }
            .theme-toggle {
                right: 5rem;
                top: 1.5rem;
            }
            .section {
                padding: 6rem 1.5rem;
            }
            .characters-container {
                width: 300px;
                height: 225px;
                transform: scale(0.8);
            }
            .logo {
                font-size: 0.9rem;
                max-width: 200px;
            }
        }
        
        /* Magnetic Effect */
        .magnetic {
            transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
    </style>
</head>
<body>
    <!-- Grain Overlay -->
    <div class="grain"></div>
    
    <!-- Stars (Night Mode Only) -->
    <div class="stars-container" id="stars"></div>
    
    <!-- Day Elements (Light Mode Only) -->
    <div class="day-elements">
        <div class="sun"></div>
        <div class="cloud"></div>
        <div class="cloud"></div>
        <div class="cloud"></div>
    </div>
    
    <!-- Custom Cursor -->
    <div class="cursor"></div>
    
    <!-- Theme Toggle -->
    <div class="theme-toggle magnetic" onclick="toggleTheme()" role="button" tabindex="0" aria-label="切换主题">
        <div class="toggle-slider">
            <span class="toggle-icon moon-icon">🌙</span>
            <span class="toggle-icon sun-icon">☀️</span>
        </div>
    </div>
    
    <!-- Navigation -->
    <nav class="nav">
        <div class="logo magnetic">让每个人都享受到科技的乐趣</div>
        <div class="menu-trigger magnetic" onclick="toggleMenu()">
            <span></span>
            <span></span>
            <span></span>
        </div>
    </nav>
    
    <!-- Fullscreen Menu -->
    <div class="fullscreen-menu" id="menu">
        <a href="#about" class="menu-item" onclick="toggleMenu()">关于</a>
        <a href="#links" class="menu-item" onclick="toggleMenu()">链接</a>
        <a href="#contact" class="menu-item" onclick="toggleMenu()">PERSON</a>
    </div>
    
    <!-- Hero Section -->
    <section class="hero" id="home">
        <div class="hero-bg"></div>
        
        <!-- Animated Characters -->
        <div class="characters-container" id="charactersContainer">
            <div class="character char-orange" data-depth="0.8">
                <div class="face">
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                    <div class="mouth"></div>
                </div>
            </div>
            <div class="character char-blue" data-depth="0.5">
                <div class="face">
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                    <div class="mouth"></div>
                </div>
            </div>
            <div class="character char-black" data-depth="0.3">
                <div class="face">
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                </div>
            </div>
            <div class="character char-yellow" data-depth="0.2">
                <div class="face">
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                    <div class="eye">
                        <div class="pupil"></div>
                    </div>
                    <div class="beak"></div>
                </div>
            </div>
        </div>
        
        <div class="scroll-indicator">
            <span class="scroll-text">Scroll</span>
            <div class="scroll-line"></div>
        </div>
    </section>
    
    <!-- About Section -->
    <section class="section" id="about">
        <div class="content-wrapper">
            <h2 class="section-title font-display">关于</h2>
            <p class="interactive-text" id="aboutText">
                这是我搭建的第一个网站，代表着我对前端开发的热爱与探索的开始。
            </p>
        </div>
    </section>
    
    <!-- Links Section -->
    <section class="section links-section" id="links">
        <div class="content-wrapper">
            <h2 class="section-title font-display">链接</h2>
            <p style="color: var(--text-secondary); margin-bottom: 2rem; transition: color 0.8s;">探索更多内容</p>
            
            <div class="links-grid">
                <a href="https://space.bilibili.com" target="_blank" class="link-card magnetic">
                    <div class="animated-border"></div>
                    <img src="https://kimi-web-img.moonshot.cn/img/raw.githubusercontent.com/25445537f2fa9f70b4e5b789750310f9f7adf81f.png" alt="Bilibili" class="link-icon">
                    <div class="link-title">Bilibili</div>
                    <div class="link-desc">观看视频内容，分享创作灵感</div>
                    <div class="link-arrow">→</div>
                </a>
                
                <a href="https://github.com" target="_blank" class="link-card magnetic">
                    <div class="animated-border"></div>
                    <img src="https://kimi-web-img.moonshot.cn/img/toppng.com/a0efdbe1d095959928549db4c5dc62afa8657cd1.png" alt="GitHub" class="link-icon">
                    <div class="link-title">GitHub</div>
                    <div class="link-desc">查看代码仓库，技术开源分享</div>
                    <div class="link-arrow">→</div>
                </a>
            </div>
        </div>
    </section>
    
    <!-- Contact/Footer Section -->
    <footer class="footer" id="contact">
        <p class="footer-text">© 2026 科技用之于民. CRAFTED WITH PASSION.</p>
    </footer>
    
    <script>
        // Generate Stars
        function generateStars() {
            const starsContainer = document.getElementById('stars');
            const starCount = 100;
            
            for (let i = 0; i < starCount; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.animationDelay = Math.random() * 3 + 's';
                star.style.animationDuration = (Math.random() * 3 + 2) + 's';
                
                // Random size variation
                const size = Math.random() * 2 + 1;
                star.style.width = size + 'px';
                star.style.height = size + 'px';
                
                starsContainer.appendChild(star);
            }
        }
        
        generateStars();
        
        // Theme Toggle Function
        function toggleTheme() {
            const html = document.documentElement;
            const currentTheme = html.getAttribute('data-theme');
            const newTheme = currentTheme === 'light' ? 'dark' : 'light';
            
            html.setAttribute('data-theme', newTheme);
            
            try {
                localStorage.setItem('theme', newTheme);
            } catch (e) {
                console.log('localStorage not available');
            }
        }
        
        // 页面加载时检查本地存储的主题偏好
        window.addEventListener('DOMContentLoaded', () => {
            try {
                const savedTheme = localStorage.getItem('theme');
                if (savedTheme) {
                    document.documentElement.setAttribute('data-theme', savedTheme);
                }
            } catch (e) {
                console.log('localStorage not available');
            }
        });
        
        // Custom Cursor
        const cursor = document.querySelector('.cursor');
        const magneticElements = document.querySelectorAll('.magnetic');
        
        document.addEventListener('mousemove', (e) => {
            cursor.style.left = e.clientX - 10 + 'px';
            cursor.style.top = e.clientY - 10 + 'px';
            
            // Magnetic effect for buttons
            magneticElements.forEach(elem => {
                const rect = elem.getBoundingClientRect();
                const x = e.clientX - rect.left - rect.width / 2;
                const y = e.clientY - rect.top - rect.height / 2;
                const distance = Math.sqrt(x * x + y * y);
                
                if (distance < 100) {
                    const magnetX = x * 0.3;
                    const magnetY = y * 0.3;
                    elem.style.transform = `translate(${magnetX}px, ${magnetY}px)`;
                } else {
                    elem.style.transform = 'translate(0, 0)';
                }
            });
        });
        
        // Cursor hover states
        const hoverElements = document.querySelectorAll('a, button, .menu-trigger, .link-card, .theme-toggle');
        hoverElements.forEach(elem => {
            elem.addEventListener('mouseenter', () => cursor.classList.add('hover'));
            elem.addEventListener('mouseleave', () => cursor.classList.remove('hover'));
        });
        
        // Menu Toggle
        function toggleMenu() {
            const menu = document.getElementById('menu');
            const trigger = document.querySelector('.menu-trigger');
            menu.classList.toggle('active');
            trigger.classList.toggle('active');
        }
        
        // Interactive Text - Word Split
        const aboutText = document.getElementById('aboutText');
        const text = aboutText.textContent;
        aboutText.innerHTML = text.split('').map(char => {
            if (char === ' ') return ' ';
            return `<span class="word">${char}</span>`;
        }).join('');
        
        // Link Card Spotlight Effect
        const linkCards = document.querySelectorAll('.link-card');
        linkCards.forEach(card => {
            card.addEventListener('mousemove', (e) => {
                const rect = card.getBoundingClientRect();
                const x = ((e.clientX - rect.left) / rect.width) * 100;
                const y = ((e.clientY - rect.top) / rect.height) * 100;
                card.style.setProperty('--x', x + '%');
                card.style.setProperty('--y', y + '%');
            });
        });
        
        // Eye Tracking Logic with Smooth Lerp
        const eyes = document.querySelectorAll('.eye');
        const pupils = [];
        
        // Initialize pupil positions
        eyes.forEach(eye => {
            const pupil = eye.querySelector('.pupil');
            const eyeRect = eye.getBoundingClientRect();
            pupils.push({
                element: pupil,
                eye: eye,
                currentX: 0,
                currentY: 0,
                targetX: 0,
                targetY: 0
            });
        });
        
        let mouseX = window.innerWidth / 2;
        let mouseY = window.innerHeight / 2;
        
        document.addEventListener('mousemove', (e) => {
            mouseX = e.clientX;
            mouseY = e.clientY;
            
            // Calculate target positions for each pupil
            pupils.forEach(pupilData => {
                const eyeRect = pupilData.eye.getBoundingClientRect();
                const eyeCenterX = eyeRect.left + eyeRect.width / 2;
                const eyeCenterY = eyeRect.top + eyeRect.height / 2;
                
                // Calculate angle and distance
                const dx = mouseX - eyeCenterX;
                const dy = mouseY - eyeCenterY;
                const angle = Math.atan2(dy, dx);
                const distance = Math.min(Math.sqrt(dx * dx + dy * dy), 3); // Limit movement range
                
                // Set target position
                pupilData.targetX = Math.cos(angle) * distance;
                pupilData.targetY = Math.sin(angle) * distance;
            });
        });
        
        // Smooth animation loop
        function animateEyes() {
            pupils.forEach(pupilData => {
                // Lerp (Linear Interpolation) for smooth movement
                const lerpFactor = 0.15; // Adjust for smoothness (0.1 = slower/smoother, 0.3 = faster)
                
                pupilData.currentX += (pupilData.targetX - pupilData.currentX) * lerpFactor;
                pupilData.currentY += (pupilData.targetY - pupilData.currentY) * lerpFactor;
                
                // Apply transform
                pupilData.element.style.transform = `translate(calc(-50% + ${pupilData.currentX}px), calc(-50% + ${pupilData.currentY}px))`;
            });
            
            requestAnimationFrame(animateEyes);
        }
        
        animateEyes();
        
        // Character parallax effect
        const characters = document.querySelectorAll('.character');
        let charMouseX = window.innerWidth / 2;
        let charMouseY = window.innerHeight / 2;
        let currentCharX = charMouseX;
        let currentCharY = charMouseY;
        
        document.addEventListener('mousemove', (e) => {
            charMouseX = e.clientX;
            charMouseY = e.clientY;
        });
        
        function animateCharacters() {
            // Smooth lerp for mouse movement
            currentCharX += (charMouseX - currentCharX) * 0.1;
            currentCharY += (charMouseY - currentCharY) * 0.1;
            
            const centerX = window.innerWidth / 2;
            const centerY = window.innerHeight / 2;
            
            characters.forEach((char, index) => {
                const depth = parseFloat(char.getAttribute('data-depth'));
                const moveX = (currentCharX - centerX) * depth * 0.05;
                const moveY = (currentCharY - centerY) * depth * 0.05;
                
                // Add slight rotation based on mouse position
                const rotateX = (currentCharY - centerY) * depth * 0.01;
                const rotateY = (currentCharX - centerX) * depth * -0.01;
                
                char.style.transform = `
                    translate(${moveX}px, ${moveY}px)
                    rotateX(${rotateX}deg)
                    rotateY(${rotateY}deg)
                    scale(${1 + Math.sin(Date.now() * 0.001 + index) * 0.02})
                `;
            });
            
            requestAnimationFrame(animateCharacters);
        }
        
        animateCharacters();
        
        // Click interaction for characters
        characters.forEach(char => {
            char.addEventListener('click', () => {
                char.classList.remove('squeezed');
                void char.offsetWidth; // Trigger reflow
                char.classList.add('squeezed');
                
                setTimeout(() => {
                    char.classList.remove('squeezed');
                }, 400);
            });
            
            char.addEventListener('mouseenter', () => {
                char.style.filter = 'brightness(1.1)';
            });
            
            char.addEventListener('mouseleave', () => {
                char.style.filter = 'brightness(1)';
            });
        });
        
        // Parallax on Scroll
        let ticking = false;
        window.addEventListener('scroll', () => {
            if (!ticking) {
                window.requestAnimationFrame(() => {
                    const scrolled = window.pageYOffset;
                    const parallax = document.querySelector('.hero-bg');
                    if (parallax) {
                        parallax.style.transform = `translateY(${scrolled * 0.5}px)`;
                    }
                    
                    ticking = false;
                });
                ticking = true;
            }
        });
        
        // Reveal on Scroll
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -100px 0px'
        };
        
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);
        
        document.querySelectorAll('.section-title, .interactive-text').forEach(el => {
            el.style.opacity = '0';
            el.style.transform = 'translateY(30px)';
            el.style.transition = 'all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1)';
            observer.observe(el);
        });
        
        // Smooth Scroll
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({
                        behavior: 'smooth',
                        block: 'start'
                    });
                }
            });
        });
        
        // Random blinking
        setInterval(() => {
            const allEyes = document.querySelectorAll('.eye');
            allEyes.forEach(eye => {
                if (Math.random() > 0.95) {
                    eye.style.transform = 'scaleY(0.1)';
                    setTimeout(() => {
                        eye.style.transform = 'scaleY(1)';
                    }, 150);
                }
            });
        }, 3000);
    </script>
</body>
</html>
