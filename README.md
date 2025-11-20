<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Painel Mod Menu Ultimate - PRO</title>
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" integrity="sha512-DTOQO9RWCH3ppGqcWaEA1BIZOC6xxalwEsw9c2QQeAIftl+Vegovlnee1c9QX4TctnWMn13TZye+giMm8e2LwA==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    
    <style>
        /* === CONFIGURAÇÕES VISUAIS (IOS NEON PRO) === */
        :root {
            --primary-color: #9d00ff; /* Roxo Neon Principal */
            --secondary-color: #39ff14; /* NOVO VERDE ELÉTRICO (Mais legível) */
            --bg-color: #000000; 
            --panel-bg-blur: rgba(255, 255, 255, 0.08); 
            --text-color: #ffffff;
            --secondary-text-color: #b0b0b0;
            --danger-color: #ff3b30; 
            --ios-radius: 12px; 
            --sutil-glow: 0 0 3px rgba(255, 255, 255, 0.3); 
        }

        /* RESET GERAL E FONTES */
        * { 
            box-sizing: border-box; 
            user-select: none; 
            scrollbar-width: none; 
            -ms-overflow-style: none;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
        }
        *::-webkit-scrollbar { display: none; }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--bg-color); 
            color: var(--text-color);
            height: 100vh;
            width: 100vw;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative; 
            font-size: 14px;
        }
        
        /* === FUNDO DE ESTRELAS ROXAS EM MOVIMENTO === */
        body::before, body::after { content: ''; position: absolute; background-repeat: repeat; pointer-events: none; z-index: -1; }
        body::before { top: 0; left: 0; width: 100%; height: 100%; background-image: radial-gradient(1px 1px at 50% 50%, #9d00ff90, transparent); background-size: 80px 80px; animation: starMove1 80s linear infinite; opacity: 0.7; }
        body::after { top: -50%; left: -50%; width: 200%; height: 200%; background-image: radial-gradient(2px 2px at 50% 50%, #9d00ff50, transparent); background-size: 150px 150px; animation: starMove2 120s linear infinite reverse; opacity: 0.5; }

        @keyframes starMove1 { from { background-position: 0 0; } to { background-position: 800px 800px; } } 
        @keyframes starMove2 { from { background-position: 0 0; } to { background-position: -1000px -1000px; } } 

        /* === PAINEL GLASSMORHPISM === */
        .panel-container {
            width: 95%; max-width: 450px; height: 90vh; max-height: 800px;
            background: rgba(255, 255, 255, 0.08); 
            backdrop-filter: blur(25px) saturate(180%); -webkit-backdrop-filter: blur(25px) saturate(180%);
            border: none; border-radius: 20px; 
            box-shadow: inset 0 0 10px rgba(157, 0, 255, 0.1), 0 8px 60px 0 rgba(0, 0, 0, 0.6); 
            display: flex; flex-direction: column; position: relative; z-index: 1; overflow: hidden; padding-bottom: 0; 
            transition: all 0.5s ease-out; /* Adiciona transição para o desaparecimento */
        }
        
        /* EFEITO LINHAS/ESTRELAS DENTRO DO PAINEL */
        .panel-container::before {
            content: ''; position: absolute; top: -50%; left: -50%; width: 200%; height: 200%;
            background: repeating-linear-gradient( 45deg, rgba(157, 0, 255, 0.1), rgba(157, 0, 255, 0.1) 1px, transparent 1px, transparent 20px );
            background-size: 20px 20px; transform: rotate(20deg); z-index: -1; animation: lineScroll 30s linear infinite; opacity: 0.8; pointer-events: none;
        }
        @keyframes lineScroll { from { background-position: 0 0; } to { background-position: 800px 800px; } }

        /* ESTADO DE CONCLUÍDO (Tela de transição) */
        .panel-container.success-hide {
            animation: panelFadeOut 0.5s forwards;
            pointer-events: none;
        }
        @keyframes panelFadeOut {
            0% { opacity: 1; transform: scale(1); }
            100% { opacity: 0; transform: scale(0.95); }
        }

        #success-overlay {
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0, 0, 0, 0.95);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 50;
            padding: 20px;
            text-align: center;
            animation: fadeIn 0.5s;
        }
        #success-overlay.show { display: flex; }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .success-icon {
            font-size: 100px;
            color: var(--secondary-color);
            margin-bottom: 20px;
            animation: shieldPulse 3s infinite ease-in-out; 
            text-shadow: 0 0 20px var(--secondary-color), 0 0 40px rgba(57, 255, 20, 0.5);
        }
        .success-text {
            font-size: 28px;
            font-weight: 700;
            color: var(--text-color);
            text-shadow: 0 0 10px var(--secondary-color);
            margin-bottom: 10px;
        }
        .success-subtext {
            color: var(--secondary-text-color);
            font-size: 16px;
        }


        /* === ABAS DE NAVEGAÇÃO COM ÍCONES REALISTAS === */
        .tabs-header {
            display: flex; 
            flex-direction: column; 
            position: absolute; 
            top: 0; 
            right: 0; 
            width: 60px; 
            height: auto; 
            background: rgba(0,0,0,0.7); 
            border-radius: 0 20px 0 20px; 
            padding: 10px 0; 
            border-left: 1px solid rgba(255, 255, 255, 0.1); 
            z-index: 10; 
        }

        .tab-btn {
            flex: none; 
            width: 100%; 
            background: none; border: none; padding: 10px 0; 
            color: var(--secondary-text-color); cursor: pointer; display: flex;
            flex-direction: column; align-items: center; font-size: 10px; 
            font-weight: 500; transition: 0.3s; border-radius: 0; 
            text-shadow: none;
            margin-bottom: 5px; 
        }
        .tab-btn:first-child { margin-top: 5px; } 
        .tab-btn:last-child { margin-bottom: 5px; } 


        .tab-btn i { 
            display: block; 
            font-size: 18px; margin-bottom: 4px; line-height: 1;
            filter: grayscale(80%) brightness(120%); 
            transition: all 0.3s ease-in-out;
            color: var(--secondary-text-color); 
        }
        .tab-btn span.tab-emoji { display: none; }
        
        .tab-btn.active i { 
            filter: none; 
            color: var(--primary-color); 
            text-shadow: 0 0 10px var(--primary-color), 0 0 15px rgba(255, 255, 255, 0.5); 
            transform: scale(1.1); 
        }
        .tab-btn.active {
            color: var(--text-color);
            background: rgba(157, 0, 255, 0.15); 
            border-left: 3px solid var(--primary-color); 
            border-radius: 0;
        }

        /* === ÁREA DE CONTEÚDO === */
        .content-body { flex: 1; padding: 20px 25px 20px 25px; overflow-y: auto; position: relative; } 
        .tab-content { display: none; flex-direction: column; gap: 15px; height: 100%; }
        .tab-content.active { display: flex; animation: fadeIn 0.3s; }
        
        /* Adicionando padding para evitar sobreposição */
        .content-body { padding-right: 70px; /* Deixa espaço para a aba de 60px + folga */ } 

        /* Cabeçalho da função */
        .function-header {
            display: flex; align-items: center; padding-bottom: 15px;
            margin-bottom: 15px; border-bottom: 1px solid rgba(255, 255, 255, 0.1); 
        }
        
        .func-header-emoji {
            font-size: 24px; margin-right: 10px; line-height: 1;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.8), 0 0 20px var(--primary-color); 
            color: var(--primary-color); 
            transition: all 0.3s ease;
        }
        .function-header span.func-header-emoji { display: block; } 
        .function-header .func-name {
            color: var(--text-color); font-weight: 600; text-transform: uppercase;
            margin-left: 5px; text-shadow: 0 0 4px var(--primary-color); 
        }
        .function-header span { color: var(--secondary-text-color); font-size: 14px; font-weight: 400; }
        
        /* === MÓDULO DE CLIQUE PERFEITO (CONTROLES) === */
        .control-row {
            background: rgba(255, 255, 255, 0.05); 
            padding: 14px; border-radius: var(--ios-radius);
            display: flex; justify-content: space-between; align-items: center;
            margin-bottom: 10px; font-size: 14px; font-weight: 500;
            transition: all 0.2s; color: var(--text-color); 
            text-shadow: var(--sutil-glow); cursor: pointer; 
            border: 1px solid transparent; 
        }
        .control-row:hover { background: rgba(255, 255, 255, 0.1); }
        
        .control-row.active {
            background: rgba(57, 255, 20, 0.15); 
            border-color: var(--secondary-color); 
            box-shadow: 0 0 10px rgba(57, 255, 20, 0.4), inset 0 0 5px rgba(57, 255, 20, 0.5); 
            color: var(--text-color); 
            text-shadow: 0 0 5px var(--secondary-color);
        }

        /* Toggle Switch */
        .switch { position: relative; display: inline-block; width: 48px; height: 28px; }
        .switch input { opacity: 0; width: 0; height: 0; } 
        .control-row .switch, .control-row .switch input, .control-row .switch .slider { pointer-events: none; }

        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #38383a; transition: .4s; border-radius: 28px; }
        .slider:before { position: absolute; content: ""; height: 24px; width: 24px; left: 2px; bottom: 2px; background-color: white; transition: .4s; border-radius: 50%; box-shadow: 0 1px 3px rgba(0,0,0,0.4); }
        
        .switch input:checked + .slider { 
            background-color: var(--secondary-color); 
            box-shadow: 0 0 10px rgba(57, 255, 20, 0.3);
        }
        .switch input:checked + .slider:before { 
            transform: translateX(20px); 
        }

        /* Range Slider */
        input[type=range] { width: 100%; height: 5px; background: #555; border-radius: 5px; outline: none; -webkit-appearance: none; margin-top: 10px; border: none; }
        input[type=range]::-webkit-slider-thumb { 
            -webkit-appearance: none; width: 28px; height: 28px; border-radius: 50%; 
            background: var(--secondary-color); cursor: pointer; 
            box-shadow: 0 0 15px rgba(57, 255, 20, 0.8), 0 0 5px rgba(0,0,0,0.5); 
        }

        /* Botões */
        .btn { width: 100%; padding: 15px; margin-bottom: 10px; background: rgba(157, 0, 255, 0.2); border: 1px solid var(--primary-color); color: white; border-radius: var(--ios-radius); font-weight: 600; cursor: pointer; transition: 0.3s; text-transform: uppercase; font-size: 14px; box-shadow: 0 4px 15px rgba(157, 0, 255, 0.1); text-shadow: var(--sutil-glow); }
        .btn:active { transform: scale(0.99); background: rgba(157, 0, 255, 0.3); }
        .btn i { margin-right: 8px; font-size: 16px; color: rgba(255, 255, 255, 0.7); } 
        .btn.selected {
            background: var(--primary-color); border-color: var(--secondary-color); 
            box-shadow: 0 0 10px rgba(57, 255, 20, 0.5); color: var(--text-color);
            text-shadow: 0 0 5px rgba(255, 255, 255, 0.7); 
        }
        .btn.selected i { color: var(--secondary-color); text-shadow: 0 0 5px var(--secondary-color); }

        .btn-action { background: var(--primary-color); border: 1px solid var(--secondary-color); box-shadow: 0 0 20px var(--primary-color); text-shadow: 0 0 8px rgba(255,255,255,0.7); margin-top: 20px; }
        .btn-action:hover { background: #7b00cc; }
        .btn-action i { color: #fff; text-shadow: 0 0 5px rgba(255, 255, 255, 0.7); }


        /* === ANIMAÇÕES PARA O ANTIBAN e GERAL === */
        @keyframes shieldPulse {
            0% { transform: scale(1); box-shadow: 0 0 10px var(--secondary-color); opacity: 0.9; }
            50% { transform: scale(1.05); box-shadow: 0 0 25px var(--secondary-color), 0 0 10px rgba(255, 255, 255, 0.8); opacity: 1; }
            100% { transform: scale(1); box-shadow: 0 0 10px var(--secondary-color); opacity: 0.9; }
        }
        
        @keyframes textGlow {
            0%, 100% { text-shadow: 0 0 8px var(--secondary-color), 0 0 1px rgba(255, 255, 255, 0.5); }
            50% { text-shadow: 0 0 15px var(--secondary-color), 0 0 5px var(--secondary-color); }
        }
        
        @keyframes scanLine {
            0% { left: -100%; }
            50% { left: 100%; }
            100% { left: -100%; }
        }


        /* ABA ANTIBAN */
        .antiban-emoji-large {
            font-size: 70px; background: rgba(57, 255, 20, 0.1); 
            padding: 20px; border-radius: 50%; margin-bottom: 10px;
            box-shadow: 0 0 15px var(--secondary-color);
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.7); 
            line-height: 1; display: inline-block;
            color: var(--secondary-color); 
            animation: shieldPulse 3s infinite ease-in-out; 
        }
        #antiban h2 { 
            font-size:24px; font-weight: 700; color:var(--secondary-color); 
            text-shadow: 0 0 8px var(--secondary-color); 
            margin-top: 10px; margin-bottom: 5px; text-transform: uppercase; 
            animation: textGlow 2.5s infinite ease-in-out; 
        }
        .antiban-details { 
            background:rgba(255, 255, 255, 0.05); padding:15px; border-radius:var(--ios-radius); border:1px solid rgba(255, 255, 255, 0.1); font-size:12px; text-shadow: var(--sutil-glow); 
            position: relative;
            overflow: hidden; 
        }
        /* Efeito de linha de varredura */
        .antiban-details::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(57, 255, 20, 0.2), transparent);
            animation: scanLine 8s infinite linear;
            pointer-events: none;
        }

        /* === MENSAGEM DE STATUS GLOBAL (POP-UP) - DISCRETO === */
        #status-message-container {
            position: fixed; 
            bottom: 70px; 
            left: 50%;
            transform: translateX(-50%);
            z-index: 100; 
            
            background: none; 
            padding: 12px 20px;
            border-radius: var(--ios-radius);
            font-size: 14px;
            font-weight: 600;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s, visibility 0.3s;
            max-width: 90%;
            text-align: center;
            
            border: none; 
            box-shadow: none; 
        }
        #status-message-container.show { opacity: 1; visibility: visible; }
        
        /* ABA INJECTOR - AJUSTADO */
        .status-card { 
            background: rgba(0,0,0,0.3); border: 1px solid #555; border-radius: var(--ios-radius); 
            padding: 20px; box-shadow: 0 0 10px rgba(255, 255, 255, 0.05); 
            display: flex; align-items: center; transition: all 0.4s; 
            min-height: 70px; 
        }
        .status-card .status-indicator { 
            width: 15px; height: 15px; background-color: var(--danger-color); 
            border-radius: 50%; box-shadow: 0 0 8px var(--danger-color); 
            margin-right: 15px; flex-shrink: 0; transition: all 0.4s; 
        }
        .status-card h3 { 
            color: #ccc; font-weight: 600; font-size: 16px; margin: 0 0 5px 0; 
            text-shadow: var(--sutil-glow); transition: all 0.4s; 
        }
        .status-card p { font-size: 11px; color: var(--secondary-text-color); }

        /* Barra de progresso */
        .progress-bar-container {
            width: 100%; height: 5px; background-color: rgba(255, 255, 255, 0.1); 
            border-radius: 5px; margin-top: 10px; overflow: hidden;
        }
        .progress-bar {
            height: 100%; width: 0%; background-color: var(--primary-color);
            transition: width 0.4s linear, background-color 0.4s; 
            box-shadow: 0 0 10px var(--primary-color);
        }

        /* Rodapé */
        .footer-info { background: rgba(0,0,0,0.6); padding: 15px 25px; font-size: 11px; border-radius: 0 0 20px 20px; border-top: 1px solid rgba(255, 255, 255, 0.1); z-index: 2; }
        .footer-row { display: flex; justify-content: space-between; margin-bottom: 5px; text-shadow: var(--sutil-glow); }
        .online-tag { color: var(--secondary-color); font-weight: 700; text-shadow: 0 0 5px var(--secondary-color); }

    </style>
</head>
<body>

    <input type="file" id="file-upload" style="display: none;" webkitdirectory directory>

    <div id="success-overlay" style="display: none; position: absolute; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0, 0, 0, 0.95); z-index: 50; flex-direction: column; justify-content: center; align-items: center; text-align: center;">
        <div class="success-icon"><i class="fas fa-check-circle"></i></div>
        <div class="success-text">SUCESSO! INJEÇÃO 100%</div>
        <div class="success-subtext" id="overlay-subtext">Comando de lançamento do jogo enviado. Mantenha o painel em segundo plano.</div>
    </div>


    <div class="panel-container" id="main-panel">
        
        <div class="tabs-header">
            <button class="tab-btn active" onclick="openTab('mira', this, 'MIRA', 'fa-crosshairs')">
                <i class="fas fa-crosshairs"></i>
                <span>MIRA</span>
            </button>
            <button class="tab-btn" onclick="openTab('pasta', this, 'PASTA', 'fa-folder-open')">
                <i class="fas fa-folder-open"></i>
                <span>PASTA</span>
            </button>
            <button class="tab-btn" onclick="openTab('antiban', this, 'PROTEÇÃO ANT', 'fa-shield-alt')">
                <i class="fas fa-shield-alt"></i>
                <span>ANTIBAN</span>
            </button>
            <button class="tab-btn" onclick="openTab('injector', this, 'INJECTOR', 'fa-syringe')">
                <i class="fas fa-syringe"></i>
                <span>INJECTOR</span>
            </button>
        </div>

        <div class="content-body">
            
            <div class="function-header">
                <span id="current-func-icon" class="func-header-emoji"><i class="fas fa-crosshairs"></i></span>
                <span>MÓDULO:</span> <span class="func-name" id="current-func-name">MIRA</span>
            </div>
            
            <div id="mira" class="tab-content active">
                <p style="font-size:12px; color:var(--secondary-text-color); margin-top:-10px; margin-bottom:15px; font-weight: 300;">Sistema de mira e estabilização avançada.</p>
                
                <div class="control-row" id="row-aimbot" onclick="handleToggle('checkbox-aimbot', 'row-aimbot', 'AIMBOT [360]')">
                    <span>AIMBOT [360]</span>
                    <label class="switch">
                        <input type="checkbox" id="checkbox-aimbot"> 
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="control-row" id="row-headshot" onclick="handleToggle('checkbox-headshot', 'row-headshot', 'HEADSHOT ASSIST')">
                    <span>HEADSHOT ASSIST</span>
                    <label class="switch">
                        <input type="checkbox" id="checkbox-headshot">
                        <span class="slider"></span>
                    </label>
                </div>
                
                <div style="margin-top:20px; padding: 10px 0;">
                    <div style="display:flex; justify-content:space-between; align-items:center;">
                        <small style="color:var(--secondary-text-color); font-weight: 500; text-shadow: none;">RAIO DE FOV DE MIRA</small>
                        <span id="fov-value" style="font-weight:700; color:var(--primary-color); text-shadow:0 0 5px var(--primary-color);">5</span>
                    </div>
                    <input type="range" id="fov-slider" min="1" max="10" value="5" step="1" oninput="updateFovValue(this.value)">
                </div>
                <div class="control-row" id="row-precisao" onclick="handleToggle('checkbox-precisao', 'row-precisao', 'PRECISÃO MÁXIMA')">
                    <span>PRECISÃO MÁXIMA</span>
                    <label class="switch">
                        <input type="checkbox" id="checkbox-precisao">
                        <span class="slider"></span>
                    </label>
                </div>
                <div class="control-row" id="row-render" onclick="handleToggle('checkbox-render', 'row-render', 'RENDERIZAÇÃO HD')">
                    <span>ATIVAR RENDERIZAÇÃO HD</span>
                    <label class="switch">
                        <input type="checkbox" id="checkbox-render">
                        <span class="slider"></span>
                    </label>
                </div>
            </div>

            <div id="pasta" class="tab-content">
                <h3 style="text-align:center; font-size:18px; color:var(--secondary-color); text-shadow: 0 0 5px var(--secondary-color); margin-top:0; margin-bottom:25px; font-weight: 600;">SELEÇÃO DE VERSÃO</h3>

                <button id="btn-ff-normal" class="btn" onclick="selectGame('com.dts.freefireth', 'FREE FIRE NORMAL', true)">
                    <i class="fas fa-mobile-alt"></i> FREE FIRE NORMAL
                </button>
                <button id="btn-ff-max" class="btn" onclick="selectGame('com.dts.freefiremax', 'FREE FIRE MAX', true)">
                    <i class="fas fa-desktop"></i> FREE FIRE MAX
                </button>

                <hr style="border:0; border-top:1px solid rgba(255, 255, 255, 0.1); width:100%; margin:20px 0;">

                <button class="btn btn-action" onclick="triggerFile()">
                    <i class="fas fa-upload"></i> INJETAR ARQUIVOS DA PASTA
                </button>
            </div>

            <div id="antiban" class="tab-content">
                <div class="antiban-content" style="text-align: center;">
                    <span class="antiban-emoji-large"><i class="fas fa-shield-virus"></i></span> 
                    <h2>PROTEÇÃO ATIVA</h2>
                    <p class="antiban-info" style="color:var(--secondary-text-color); font-size:13px;">Sistema de proteção em tempo real está totalmente operacional.</p> 
                    <div class="antiban-details">
                        <div class="footer-row">
                            <span>BYPASS DE SEGURANÇA:</span> 
                            <span style="color:var(--secondary-color); font-weight: 600; text-shadow: 0 0 5px var(--secondary-color);">ATIVADO</span>
                        </div>
                        <div class="footer-row">
                            <span>BLOQUEIO DE REPORT:</span> 
                            <span style="color:var(--secondary-color); font-weight: 600; text-shadow: 0 0 5px var(--secondary-color);">ATIVADO</span>
                        </div>
                    </div>
                </div>
            </div>

            <div id="injector" class="tab-content">
                <h3 style="text-align:center; font-size:18px; color:var(--secondary-color); text-shadow: 0 0 5px var(--secondary-color); margin-top:0; margin-bottom:25px; font-weight: 600;">STATUS DA INJEÇÃO</h3>

                <div class="status-card" id="status-panel">
                    <div class="status-indicator" id="injector-status-indicator"></div>
                    <div class="status-text-content" style="flex-grow: 1;">
                        <h3 id="status-text">AGUARDANDO INJEÇÃO...</h3> 
                        <p id="status-desc">Nenhum processo foi iniciado.</p>
                        <div class="progress-bar-container">
                            <div class="progress-bar" id="injection-progress-bar"></div>
                        </div>
                    </div>
                </div>

                <button class="btn" onclick="startInjection('com.dts.freefireth', 'FREE FIRE NORMAL')">
                    <i class="fas fa-download"></i> INJETAR FF NORMAL
                </button>
                <button class="btn" onclick="startInjection('com.dts.freefiremax', 'FREE FIRE MAX')">
                    <i class="fas fa-download"></i> INJETAR FF MAX
                </button>
            </div>

        </div>

        <div class="footer-info">
            <div class="footer-row">
                <span>STATUS DO SERVIDOR: <span class="online-tag">ONLINE</span></span>
                <span>VERSÃO: 1.0.0.2.5V</span>
            </div>
            <div class="footer-row" style="margin-bottom: 0;">
                <span>DESENVOLVEDOR: RzzX</span>
                <span>DATA: 19/11/25</span>
            </div>
        </div>

    </div>
    
    <div id="status-message-container"></div>


    <script>
        
        let statusTimeout;
        let selectedGamePackage = null; 
        let selectedGameName = '';
        let injectionInterval;
        
        // NOVO: Variável de controle da pasta
        let pasta_carregada_com_sucesso = false; 

        function showStatusMessage(message, color) {
            const messageContainer = document.getElementById('status-message-container');
            clearTimeout(statusTimeout);
            
            messageContainer.innerText = message;
            messageContainer.style.color = color;
            messageContainer.style.boxShadow = `none`; 
            messageContainer.style.borderColor = `transparent`; 
            messageContainer.style.textShadow = `0 0 5px ${color}`; 
            messageContainer.style.background = `none`; 
            messageContainer.classList.add('show');

            statusTimeout = setTimeout(() => {
                messageContainer.classList.remove('show');
            }, 2500); 
        }

        function handleToggle(checkboxId, rowId, functionName) {
            const checkbox = document.getElementById(checkboxId);
            const row = document.getElementById(rowId);
            
            checkbox.checked = !checkbox.checked;
            const isActive = checkbox.checked;

            if (isActive) {
                row.classList.add('active');
            } else {
                row.classList.remove('active');
            }
            
            const statusText = isActive ? "ATIVADA" : "DESATIVADA";
            const statusColor = isActive ? "var(--secondary-color)" : "var(--danger-color)"; 

            showStatusMessage(`${functionName} - ${statusText}`, statusColor);
        }
        
        function openTab(tabId, element, funcName, funcIconName) {
            const contentElements = document.querySelectorAll('.tab-content');
            const buttonElements = document.querySelectorAll('.tab-btn');
            const targetContent = document.getElementById(tabId);
            const currentFuncIconSpan = document.getElementById('current-func-icon');
            const currentFuncName = document.getElementById('current-func-name');

            contentElements.forEach(c => c.classList.remove('active'));
            buttonElements.forEach(b => b.classList.remove('active'));
            
            if (targetContent) targetContent.classList.add('active');
            if (element) element.classList.add('active');

            if (currentFuncIconSpan && funcIconName) {
                currentFuncIconSpan.innerHTML = `<i class="fas ${funcIconName}"></i>`;
            }
            if (currentFuncName) currentFuncName.innerText = funcName;
        }

        function updateFovValue(value) {
            const fovDisplay = document.getElementById('fov-value');
            if (fovDisplay) {
                fovDisplay.innerText = value;
            }
        }
        
        function selectGame(packageName, gameName, showStatus = false) {
            selectedGamePackage = packageName;
            selectedGameName = gameName;

            const btnNormal = document.getElementById('btn-ff-normal');
            const btnMax = document.getElementById('btn-ff-max');

            if (btnNormal) btnNormal.classList.remove('selected');
            if (btnMax) btnMax.classList.remove('selected');

            if(packageName.includes('freefireth') && btnNormal) btnNormal.classList.add('selected');
            if(packageName.includes('freefiremax') && btnMax) btnMax.classList.add('selected');

            if (showStatus) {
                showStatusMessage(`Versão ${gameName} SELECIONADA.`, "var(--primary-color)");
            }
        }

        // NOVO: Função para disparar a abertura da janela de seleção de arquivo/diretório
        function triggerFile() {
            const fileUpload = document.getElementById('file-upload');
            if (fileUpload) {
                fileUpload.click();
            }
            showStatusMessage("Aguardando seleção da pasta...", "var(--primary-color)");
        }

        // NOVO: Listener para lidar com a seleção de pasta e atualizar a variável de controle
        document.getElementById('file-upload').addEventListener('change', function(event) {
            const files = event.target.files;
            if (files.length > 0) {
                pasta_carregada_com_sucesso = true; 
                showStatusMessage(`✅ PASTA CARREGADA. ${files.length} arquivos prontos para Injeção.`, "var(--secondary-color)");
            } else {
                pasta_carregada_com_sucesso = false; 
                showStatusMessage("Seleção cancelada. Injeção bloqueada.", "var(--danger-color)");
            }
        });


        /* === FUNÇÃO DE INJEÇÃO (COM VALIDAÇÃO DE PASTA) === */
        function startInjection(packageName, gameName) {
            
            if (injectionInterval) {
                clearInterval(injectionInterval);
            }
            
            const indicator = document.getElementById('injector-status-indicator');
            const text = document.getElementById('status-text');
            const desc = document.getElementById('status-desc');
            const progressBar = document.getElementById('injection-progress-bar');
            const mainPanel = document.getElementById('main-panel');
            const successOverlay = document.getElementById('success-overlay');
            const overlaySubtext = document.getElementById('overlay-subtext');
            
            // NOVO: VALIDAÇÃO DE DEPENDÊNCIA
            if (!pasta_carregada_com_sucesso) {
                indicator.style.backgroundColor = 'var(--danger-color)';
                indicator.style.boxShadow = '0 0 8px var(--danger-color)';
                progressBar.style.width = '0%';
                progressBar.style.backgroundColor = 'var(--danger-color)';
                text.innerText = "🛑 RECURSO CRÍTICO NÃO CARREGADO";
                desc.innerText = "Carregue a Pasta/Taxa na aba PASTA antes de Injetar.";
                
                showStatusMessage(`❌ INJEÇÃO CANCELADA! Carregue a Pasta/Taxa.`, "var(--danger-color)");
                return; 
            }

            selectedGamePackage = packageName; 
            selectedGameName = gameName;
            
            const progressSteps = [5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100];
            const statusStates = [
                { percent: 5, text: "Preparando ambiente...", color: "var(--primary-color)" },
                { percent: 10, text: "INICIANDO VARREDURA ANTIBAN...", color: "var(--secondary-color)" },
                { percent: 15, text: "Ativando Modulo RTT...", color: "var(--secondary-color)" },
                { percent: 20, text: "BYPASS DE SEGURANÇA: Ativando KERNEL I/O.", color: "var(--primary-color)" },
                { percent: 25, text: "Verificando integridade...", color: "var(--primary-color)" },
                { percent: 30, text: "BLOQUEIO DE TRAÇOS Ativo (30%).", color: "var(--secondary-color)" },
                { percent: 35, text: "Análise de Arquivos OBB.", color: "var(--primary-color)" },
                { percent: 40, text: "Injeção de Módulo: Anti-Report.", color: "var(--secondary-color)" },
                { percent: 45, text: "Injeção de Módulo: Aim-Assist.", color: "var(--secondary-color)" },
                { percent: 50, text: "Processo em 50%. Estabilizando...", color: "var(--primary-color)" },
                { percent: 55, text: "Injetando Modulos CRÍTICOS (55%).", color: "var(--secondary-color)" },
                { percent: 60, text: "Ajustando sensibilidade...", color: "var(--primary-color)" },
                { percent: 65, text: "Desvio de segurança concluído.", color: "var(--secondary-color)" },
                { percent: 70, text: "Injeção de Módulo: RENDER HD.", color: "var(--primary-color)" },
                { percent: 75, text: "Injeção de Módulo: FOV Máximo.", color: "var(--primary-color)" },
                { percent: 80, text: "SINCRONIZANDO dados finais...", color: "var(--secondary-color)" },
                { percent: 85, text: "Otimizando recursos (85%).", color: "var(--primary-color)" },
                { percent: 90, text: "Finalizando Injeção - 90%.", color: "var(--secondary-color)" },
                { percent: 95, text: "Limpeza de rastros finalizada.", color: "var(--primary-color)" },
                { percent: 100, text: `INJEÇÃO CONCLUÍDA! Lançando o ${gameName}...`, color: "var(--secondary-color)" } 
            ];

            // 1. Resetar e Iniciar
            indicator.style.backgroundColor = 'var(--primary-color)';
            indicator.style.boxShadow = '0 0 8px var(--primary-color)';
            text.innerText = `INICIANDO INJEÇÃO: ${gameName}`;
            desc.innerText = "Verificação de dependência concluída. Executando etapas...";
            progressBar.style.width = '0%';
            progressBar.style.backgroundColor = 'var(--primary-color)';

            let stepIndex = 0;
            
            injectionInterval = setInterval(() => {
                const currentProgress = progressSteps[stepIndex];
                progressBar.style.width = `${currentProgress}%`;

                const currentState = statusStates.find(s => s.percent === currentProgress);
                if (currentState) {
                    text.innerText = currentState.text;
                    progressBar.style.backgroundColor = currentState.color;
                }
                
                if (currentProgress === 100) {
                    desc.innerText = "Processo finalizado com sucesso. Otimização aplicada.";
                    clearInterval(injectionInterval); 
                    indicator.style.backgroundColor = 'var(--secondary-color)';
                    indicator.style.boxShadow = '0 0 8px var(--secondary-color)';
                    
                    // 1. O painel desaparece.
                    mainPanel.classList.add('success-hide');
                    
                    // 2. A tela de sucesso aparece.
                    overlaySubtext.innerText = `O jogo ${gameName} foi injetado com 100% de sucesso. Tentando abrir o aplicativo...`;
                    successOverlay.style.display = 'flex';
                    
                    // 3. TENTA LANÇAR O APLICATIVO REALMENTE APÓS UM PEQUENO ATRASO
                    setTimeout(() => {
                        const appPackage = selectedGamePackage; 
                        
                        // Comando de Intent (para dispositivos móveis)
                        const intentUri = `intent://` + appPackage + `#Intent;scheme=launch;package=` + appPackage + `;end`;
                        
                        window.location.href = intentUri; 

                        overlaySubtext.innerText = `Comando de lançamento do ${gameName} enviado! O jogo deve abrir em instantes.`;

                    }, 1500); 
                    
                    return; 
                }

                stepIndex++;
            }, 300); 
        }

    </script>
</body>
</html>
