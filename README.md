<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>龍潭大池埤塘數位資料庫</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <style>
        /* ==================== 核心全域樣式（網頁暗色系美化） ==================== */
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: "Helvetica Neue", Arial, "Noto Sans TC", sans-serif; }
        body { background-color: #0f172a; color: #f1f5f9; line-height: 1.6; }
        a { text-decoration: none; color: inherit; }

        /* 頁首 */
        .main-header { background-color: #1e293b; padding: 20px; text-align: center; border-bottom: 3px solid #0284c7; }
        .main-header .logo { font-size: 1.6rem; font-weight: bold; color: #38bdf8; }

        /* 導覽列與下拉選單 */
        .main-nav { background-color: #1e293b; border-bottom: 1px solid #334155; position: sticky; top: 0; z-index: 100; }
        .nav-list { display: flex; justify-content: center; list-style: none; flex-wrap: wrap; }
        .nav-list > li { position: relative; }
        .nav-list > li > a { display: block; padding: 15px 20px; font-weight: 500; color: #94a3b8; transition: all 0.3s; }
        .nav-list > li > a:hover { color: #38bdf8; background-color: #0f172a; }
        
        .dropdown:hover .dropdown-menu { display: block; }
        .dropdown-menu { display: none; position: absolute; top: 100%; left: 0; background-color: #1e293b; list-style: none; min-width: 200px; border: 1px solid #334155; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.5); }
        .dropdown-menu a { display: block; padding: 12px 16px; color: #94a3b8; font-size: 0.95rem; }
        .dropdown-menu a:hover { background-color: #0f172a; color: #38bdf8; }

        /* 主要內容分頁控制 */
        .main-content { max-width: 1200px; margin: 30px auto; padding: 0 20px; min-height: 70vh; }
        .page-section { display: none; } /* 預設全部隱藏 */
        .page-section.active { display: block; animation: fadeIn 0.4s ease-in-out; } /* 只顯示被啟動的分頁 */

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 看板與卡片樣式 */
        .welcome-box { background-color: #1e293b; padding: 30px; border-radius: 12px; border: 1px solid #334155; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); margin-bottom: 25px; }
        .welcome-img { width: 100%; max-height: 400px; object-fit: cover; border-radius: 8px; margin: 20px 0; border: 1px solid #475569; }
        .text-card { border-left: 5px solid #38bdf8; }
        .author { float: right; font-size: 0.9rem; color: #64748b; background-color: #0f172a; padding: 2px 8px; border-radius: 4px; }
        .article-p { margin-bottom: 15px; color: #cbd5e1; font-size: 1.05rem; text-align: justify; }
        
        /* 生態卡片網格 */
        .section-title-box { margin-bottom: 25px; border-bottom: 2px solid #334155; padding-bottom: 10px; }
        .section-title-box h2 { color: #38bdf8; }
        .card-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 25px; margin-top: 20px; }
        .bird-card { background-color: #1e293b; border-radius: 12px; overflow: hidden; border: 1px solid #334155; transition: transform 0.3s; display: flex; flex-direction: column; }
        .bird-card:hover { transform: translateY(-5px); box-shadow: 0 10px 20px rgba(0,0,0,0.5); }
        .thumb-wrapper { width: 100%; height: 200px; background-color: #0f172a; overflow: hidden; }
        .thumb-wrapper img { width: 100%; height: 100%; object-fit: cover; }
        .card-body { padding: 20px; flex-grow: 1; display: flex; flex-direction: column; justify-content: space-between; }
        .card-body h3 { color: #f8fafc; margin-bottom: 10px; font-size: 1.25rem; }
        .card-text { color: #94a3b8; font-size: 0.95rem; margin-bottom: 15px; text-align: justify; }
        .cta-btn { background-color: #0284c7; color: white; border: none; padding: 10px 15px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; text-align: center; transition: background 0.2s; }
        .cta-btn:hover { background-color: #0369a1; }

        /* 水質與昆蟲專屬風格美化 */
        .water-grid { display: grid; grid-template-columns: 1fr; gap: 20px; margin-bottom: 30px; }
        @media (min-width: 768px) { .water-grid { grid-template-columns: 1fr 1fr; } }
        .water-card { background-color: #1e293b; border: 1px solid #334155; border-radius: 12px; padding: 24px; }
        .water-card h3 { color: #38bdf8; font-size: 1.2rem; margin-bottom: 16px; border-bottom: 1px solid #334155; padding-bottom: 8px; }
        .water-list { list-style: none; }
        .water-list li { font-size: 0.95rem; color: #cbd5e1; margin-bottom: 14px; }
        .water-list li strong { color: #38bdf8; font-size: 1rem; }

        /* 地圖、表格與問答互動樣式 */
        .map-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; border-radius: 8px; border: 1px solid #334155; }
        .map-container iframe { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
        .struc-table { width: 100%; border-collapse: collapse; margin-top: 15px; background-color: #1e293b; }
        .struc-table th, .struc-table td { border: 1px solid #334155; padding: 12px; text-align: left; }
        .struc-table th { background-color: #0f172a; color: #38bdf8; }
        
        /* 融合暗色系互動任務設計 */
        .quiz-item { background-color: #0f172a; padding: 18px; border-radius: 8px; margin-bottom: 20px; border: 1px solid #334155; }
        .quiz-title { font-weight: bold; font-size: 1.05rem; margin-bottom: 12px; color: #f1f5f9; }
        
        .option-btn { display: block; width: 100%; text-align: left; background: #1e293b; border: 1px solid #475569; color: #cbd5e1; padding: 12px 16px; margin-bottom: 8px; border-radius: 6px; cursor: pointer; font-size: 0.95rem; transition: all 0.2s ease; }
        .option-btn:hover { background: #334155; border-color: #38bdf8; color: #fff; }
        .option-btn.correct { background-color: #065f46; border-color: #10b981; color: #ecfdf5; font-weight: bold; }
        .option-btn.wrong { background-color: #991b1b; border-color: #ef4444; color: #fef2f2; }
        
        .analysis-box { margin-top: 10px; padding: 12px; background-color: #0c4a6e; border-left: 4px solid #0284c7; border-radius: 4px; display: none; font-size: 0.9rem; color: #e0f2fe; }
        
        .fill-blank-input { border: none; border-bottom: 2px solid #38bdf8; padding: 2px 6px; font-size: 1rem; width: 100px; text-align: center; outline: none; background: transparent; color: #38bdf8; font-weight: bold; }
        .check-btn { background-color: #0284c7; color: white; border: none; padding: 8px 16px; border-radius: 4px; cursor: pointer; font-size: 0.9rem; margin-top: 10px; font-weight: bold; transition: background 0.2s; }
        .check-btn:hover { background-color: #0369a1; }
        .result-text { margin-left: 15px; font-weight: bold; font-size: 1rem; }
        
        .text-area-answer { width: 100%; height: 80px; background-color: #0f172a; border: 1px solid #475569; border-radius: 6px; padding: 10px; color: #f1f5f9; margin-top: 8px; box-sizing: border-box; resize: vertical; font-family: inherit; }
        .reference-answer { margin-top: 10px; background: #7c2d12; padding: 12px; border-radius: 5px; border-left: 4px solid #ea580c; display: none; font-size: 0.9rem; color: #ffedd5; }

        /* 暗色系流程圖樣式 */
        .flowchart { background: #0f172a; padding: 20px; border-radius: 8px; text-align: center; font-weight: bold; margin: 20px 0; border: 1px solid #334155; display: flex; flex-wrap: wrap; justify-content: center; align-items: center; gap: 8px; }
        .flow-step { background: #1e293b; padding: 8px 14px; border-radius: 6px; border: 1px solid #475569; font-size: 0.9rem; color: #38bdf8; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.2); }
        .arrow { color: #64748b; font-size: 1.1rem; }

        /* 原始結構與內建 details 樣式保留 */
        details { margin-top: 10px; background-color: #1e293b; padding: 10px; border-radius: 4px; border: 1px solid #475569; }
        summary { cursor: pointer; color: #38bdf8; font-weight: bold; }

        /* 彈出視窗 Modal */
        .custom-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.8); z-index: 1000; justify-content: center; align-items: center; padding: 20px; }
        .modal-content { background-color: #1e293b; padding: 30px; border-radius: 12px; max-width: 600px; width: 100%; border: 1px solid #475569; position: relative; color: #e2e8f0; }
        .modal-content h2 { color: #38bdf8; margin-bottom: 10px; }
        .modal-content hr { border: 0; border-top: 1px solid #334155; margin-bottom: 15px; }
        .modal-content p { margin-bottom: 12px; }
        .close-btn { position: absolute; top: 15px; right: 20px; font-size: 2rem; color: #94a3b8; cursor: pointer; }
        .close-btn:hover { color: #f1f5f9; }

        /* 頁尾 */
        .main-footer { background-color: #1e293b; text-align: center; padding: 20px; border-top: 1px solid #334155; color: #64748b; font-size: 0.9rem; margin-top: 50px; }
    </style>
</head>
<body>

    <header class="main-header">
        <div class="logo">
            <i class="fa-solid fa-water"></i> 龍潭大池埤塘數位資料庫
        </div>
    </header>

    <nav class="main-nav">
        <ul class="nav-list">
            <li><a href="javascript:void(0)" onclick="showPage('welcome')">關於</a></li>
            <li><a href="javascript:void(0)" onclick="showPage('map-page')">地圖</a></li>
            
            <li class="dropdown">
                <a href="javascript:void(0)">歷史與文化 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('hist-origin')">起源故事 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('hist-life')">周邊生活 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('hist-name')">地名考證 (劉亞蓁)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">生態調查 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('eco-plant')">植物 (張佳琪)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('eco-animal')">動物 (張佳琪)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('eco-bird')">鳥類 (張佳琪)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">昆蟲與水質 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('eco-insect')">昆蟲 (張佳琪)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('eco-water')">水質 (張佳琪)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">地景與水利 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('water-sys')">地形與水源 (李芷嫻)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('water-struc')">關鍵構造 (李芷嫻)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('water-logic')">分水邏輯 (李芷嫻)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">變遷與現狀 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('now-land')">土地變化 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('now-func')">轉型功能 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('now-issue')">面臨挑戰 (劉亞蓁)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">互動任務 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('task-choice')">環境選擇題 (李芷嫻)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('task-fill')">填空與思考題 (李芷嫻)</a></li>
                </ul>
            </li>
        </ul>
    </nav>

    <main class="main-content">

        <section id="welcome" class="page-section active">
            <div class="welcome-box">
                <h2>關於系統：歡迎來到龍潭大池數位資料庫</h2>
                <img src="data:image/jpeg;base64,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"alt="大池夜景">

                <p>本資料庫整合了龍潭大池的地景水利、歷史文化變遷以及豐富的埤塘生態調查資料。請點擊上方選單瀏覽各個專題頁面。</p>
            </div>
        </section>

        <section id="map-page" class="page-section">
            <div class="welcome-box">
                <h2>大池地理位置</h2>
                <div class="map-container">
                    <iframe src="https://maps.google.com/maps?q=龍潭大池&t=&z=15&ie=UTF8&iwloc=&output=embed" style="border:0;" allowfullscreen="" loading="lazy"></iframe>
                </div>
                <p style="margin-top: 15px; color: #8a99a6;"><i class="fa-solid fa-location-dot"></i> 地理標誌：桃園市龍潭區核心生態埤塘。</p>
            </div>
        </section>

        <section id="eco-plant" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：植物 (張佳琪)</h2>
                <p>探索棲息於龍潭大池的六大核心水生與挺水指標植物（點擊按鈕查看詳細紀錄）</p>
            </div>
            <div class="card-grid">
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/7/7a/Yellowwaterlili.jpg" alt="台灣萍蓬草"></div>
                    <div class="card-body">
                        <h3>台灣萍蓬草</h3>
                        <p class="card-text">台灣特有種的浮葉植物，也是大池生態復育的重點指標。全年開黃色小花，極具保育價值。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-1')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/82/Typha-Orientalis.jpg/960px-Typha-Orientalis.jpg" alt="香蒲"></div>
                    <div class="card-body">
                        <h3>香蒲</h3>
                        <p class="card-text">常見的多年生挺水植物，因其花序形似蠟燭，又俗稱「水蠟燭」，能有效淨化埤塘水質。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-2')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/Starr_061223-2773_Cyperus_involucratus.jpg/250px-Starr_061223-2773_Cyperus_involucratus.jpg" alt="風車草"></div>
                    <div class="card-body">
                        <h3>風車草</h3>
                        <p class="card-text">又稱輪傘草，莖頂叢生線形苞片如小風車。生命力極強，是護岸與挺水綠帶的主力植栽。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-3')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/36/Fleur_de_lotus.jpg/250px-Fleur_de_lotus.jpg" alt="蓮"></div>
                    <div class="card-body">
                        <h3>蓮</h3>
                        <p class="card-text">著名的挺水植物，夏季盛開。大池部分水域與人工景觀池有栽培，具備極高的觀賞與生態價值。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-4')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/Vaginalis.jpg/250px-Vaginalis.jpg" alt="鴨舌草"></div>
                    <div class="card-body">
                        <h3>鴨舌草</h3>
                        <p class="card-text">埤塘常見的原始挺水植物，葉片先端漸尖形似鴨舌。開藍紫色小花，常見於岸邊濕地。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-5')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Eichhornia_crassipes_201510.jpg/250px-Eichhornia_crassipes_201510.jpg" alt="布袋蓮"></div>
                    <div class="card-body">
                        <h3>布袋蓮</h3>
                        <p class="card-text">侵略性強的漂浮植物。過度繁殖會覆蓋大池水面、阻絕陽光，是水利維護需定期清除的物種。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-6')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
            </div>
        </section>

        <section id="eco-animal" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：動物 (張佳琪)</h2>
                <p>探索棲息於龍潭大池周邊、陸域草叢與兩棲水域的指標性動物</p>
            </div>
            <div class="card-grid">
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/Ocadia_sinensis.jpg/250px-Ocadia_sinensis.jpg" alt="斑龜"></div>
                    <div class="card-body">
                        <h3>斑龜</h3>
                        <p class="card-text">台灣最常見的本土原生爬行動物，頭部與頸部有顯眼的黃綠色細線條。常在晴天時爬上大池的木棧道或石塊上曬太陽。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-1')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/33/Takydromus_stejnegeri_166585190.jpg/250px-Takydromus_stejnegeri_166585190.jpg" alt="蓬萊草蜥"></div>
                    <div class="card-body">
                        <h3>蓬萊草蜥</h3>
                        <p class="card-text">台灣特有種蜥蜴，身體細長，尾巴極長。常出沒在大池周邊未硬化的草叢與低矮灌木叢中，主要捕食小型昆蟲。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-2')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fb/Rana_guentheri.jpg/250px-Rana_guentheri.jpg" alt="貢德氏赤蛙"></div>
                    <div class="card-body">
                        <h3>貢德氏赤蛙</h3>
                        <p class="card-text">大型兩棲類，叫聲如同狗吠「狂、狂」，極具辨識度。在大池周邊的挺水植物根部與濕軟泥土中非常活躍。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-3')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Procambarus_clarkii_top.jpg/250px-Procambarus_clarkii_top.jpg" alt="克氏原螯蝦"></div>
                    <div class="card-body">
                        <h3>克氏原螯蝦</h3>
                        <p class="card-text">惡名昭彰的外來種，雙螯強壯呈鮮紅色。牠們會在大池土堤邊掘洞，破壞護岸結構，並嚴重剪食本土水生植物的嫩芽。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-4')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Duttaphrynus_melanostictus_at_Niaosong_Wetland_Park%2C_Kaohsiung_2022-01-23_%28cropped%29.jpg/250px-Duttaphrynus_melanostictus_at_Niaosong_Wetland_Park%2C_Kaohsiung_2022-01-23_%28cropped%29.jpg" alt="黑眶蟾蜍"></div>
                    <div class="card-body">
                        <h3>黑眶蟾蜍</h3>
                        <p class="card-text">眼眶周圍有一圈明顯的黑色角質硬眶，皮膚佈滿癩蛤蟆疙瘩。多在夜間於環池步道、路燈下的草叢區覓食小型昆蟲。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-5')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Ornate_Pygmy_Frog_%28Microhyla_fissipes%29.jpg/250px-Ornate_Pygmy_Frog_%28Microhyla_fissipes%29.jpg" alt="小雨蛙"></div>
                    <div class="card-body">
                        <h3>小雨蛙</h3>
                        <p class="card-text">體型小巧，背部有美麗的三角形或類文字斑紋。每逢大雨過後的春夏夜晚，大池周邊的草叢與積水處便會傳出牠們密集的宏亮鳴叫聲。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-6')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
            </div>
        </section>

        <section id="eco-bird" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：鳥類 (張佳琪)</h2>
                <p>探索棲息並常駐於龍潭大池的六大核心指標鳥類</p>
            </div>
            <div class="card-grid">
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Gallinula_chloropus_-Westerpark_-Amsterdam-8.jpg/250px-Gallinula_chloropus_-Westerpark_-Amsterdam-8.jpg" alt="紅冠水雞"></div>
                    <div class="card-body">
                        <h3>紅冠水雞</h3>
                        <p class="card-text">大池最常見的指標性留鳥，身體黑褐色，額頭具有鮮紅色的額甲，擅長在挺水植物間穿梭與築巢。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-1')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/Litle_Grebe_-_Tachybaptus_ruficollis.jpg/250px-Litle_Grebe_-_Tachybaptus_ruficollis.jpg" alt="小鸊鷉"></div>
                    <div class="card-body">
                        <h3>小鸊鷉</h3>
                        <p class="card-text">體型小巧如鴨，是埤塘中的潛水高手。遇到驚嚇時會瞬間潛入水中，只在水面留下一陣漣漪。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-2')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/Great_Egret_%28Egretta_alba%29%2C_Skala_Kallonis%2C_Lesvos%2C_Greece%2C_19.04.2015_%2817449816992%29.jpg/250px-Great_Egret_%28Egretta_alba%29%2C_Skala_Kallonis%2C_Lesvos%2C_Greece%2C_19.04.2015_%2817449816992%29.jpg" alt="大白鷺"></div>
                    <div class="card-body">
                        <h3>大白鷺</h3>
                        <p class="card-text">優雅的涉禽，身形修長且全身披著潔白羽毛，常佇立於大池淺水灘中靜止不動，伺機捕食魚蝦。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-3')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ce/Black-crowned_Night_Heron_--_Nycticorax_nycticorax.jpg/330px-Black-crowned_Night_Heron_--_Nycticorax_nycticorax.jpg" alt="夜鷺"></div>
                    <div class="card-body">
                        <h3>夜鷺</h3>
                        <p class="card-text">俗稱「夜婆」，頭頂與背部羽毛呈藍黑色。屬於大池生態鏈的高級掠食者，多在黃昏及夜間活躍。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-4')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/Alcedo_Atthis.jpg/250px-Alcedo_Atthis.jpg" alt="翠鳥"></div>
                    <div class="card-body">
                        <h3>翠鳥</h3>
                        <p class="card-text">擁有亮麗翠藍的羽色，俗稱魚狗。常靜停在池畔木棧道或樹枝上，隨後以高速衝入水中精準啣魚。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-5')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/23/Egretta_garzetta_EM1B0094_%2847336068462%29.jpg/330px-Egretta_garzetta_EM1B0094_%2847336068462%29.jpg" alt="小白鷺"></div>
                    <div class="card-body">
                        <h3>小白鷺</h3>
                        <p class="card-text">嘴黑、腳黑但腳趾為顯眼的黃綠色。覓食時會用腳攪動泥沙，驚嚇隱藏的小魚以利捕食。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-6')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
            </div>
        </section>

        <section id="eco-insect" class="page-section">
    <div class="section-title-box">
        <h2>生態調查：昆蟲 (張佳琪)</h2>
        <p>探索棲息於龍潭大池周邊濕地、草地與水域環境的六大指標性昆蟲</p>
    </div>

    <div class="card-grid">

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="臺灣大鍬形蟲">
            </div>
            <div class="card-body">
                <h3>臺灣大鍬形蟲</h3>
                <p class="card-text">臺灣特有種鍬形蟲，雄蟲具有大型彎曲大顎，是臺灣代表性的保育昆蟲。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-1')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="纖粉蝶">
            </div>
            <div class="card-body">
                <h3>纖粉蝶</h3>
                <p class="card-text">臺灣體型最小的粉蝶之一，飛行姿態輕盈優雅。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-2')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="大草螽">
            </div>
            <div class="card-body">
                <h3>大草螽</h3>
                <p class="card-text">全身翠綠色，具有極佳的保護色與跳躍能力。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-3')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAkGBxISDxUTEhEVFhIVFxYZFRcYFhUYFRgYFxUWFhgRFRcYHSggGBoxHRYVITEhJSk3Li4uFx8zODMtNyguLisBCgoKDg0OGxAQGi0lHyUrLS8tLS0tLS0tLS0tLS0rLS0vLS0tLS0tLSstLSsuKy0tLS0tLS0tLS0tLS0tLS0tMf/AABEIALcBEwMBIgACEQEDEQH/xAAcAAEAAAcBAAAAAAAAAAAAAAAAAgMEBQYHCAH/xAA/EAACAQIDBQUFBQcDBQEAAAAAAQIDEQQSIQUGMUFhBxMiUXFSgZGhsRQjMjNyQmKCksHR8BWDsiRzosLhCP/EABcBAQEBAQAAAAAAAAAAAAAAAAACAQP/xAAgEQEBAAICAgMBAQAAAAAAAAAAAQIRITEDEkFRcTIi/9oADAMBAAIRAxEAPwDeIAAAAAAAAAAAAAAAAAAp61RxnH2XeL9XZpv4SXrJFQSsVSzQcU7NrR+T5S+NmQ4PEKcFJdU15Si3GUfdJNe4wTwAaB42elPtGtko1JpNuMJNJcXaLdkBOpyuk/NJ/H0IiCjDLFRXBJL4KxGAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAeMoqM8leUXJWqLPBc7xtGouq/A/4mVxR7SwznFOGXvIPNTcr2vqmnbgnFyi3+8ZSKwFNgsWqkb2tJO04tq8JWu4u3qn1TT5k6pVjFXk0l5t2XzNEZRbVs4KLbWecI6cX4k5L+VS91yrjNNXWqLZVxMZ4mEc8bU3LTS8qmT8Mf0wk27e2uplbF1QPEemsAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAApaOPpTnOnGpGU6dlOKknKN1dZlxWhUs0N2gzq4Ha2IqwqtutBWaa7xKdPu5QfC1lFNdMvPUjLL1Vjj7Mi7Re0GnhqieCkniFeFSdr08lnaNv25KVmnwV5cbtGpdqb44vESvWxNSfknLwr0itF7kWjHYpvTh9Sivfl/nmZJvtv4y7ZG/2Pw9lSxVTKuEZPPC3llldJehcN6t4oYjD4SpRjOniMNGSqNWScnKNT7RGS1zOak3pe8uZgMnb/AD5Fz2djrNZoqS8r2vpazuZlPpuN1XVe7G0pV8PCcozu4xblKKgpNrXKk3p14F4MZ3E27h8XhYujmi4JKVOU5TceStKXGOmj6GTF43cRlNUABTAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA8YFs3l2tHCYWpXlZ5F4V5yekV8TlXeHa1SvWlUnNylJt3fO/E3D267aShDDxlZrxz6tq0Y/ByfvRoipqzl3l+OnWP6gbuTIkMUT6dHMtCmKeSPIvUjmtSUwxmvZzvPPCYqEk/DwlH2o/tR/r6o6gw9aM4RnF3jJJp+aaumcY4Splknfhw6anT3ZRtdV9nQg3edHwy6ptuD+Gn8JmPGX6q847+maAA6OYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAASsXXjTpynJ2jCLlL0irv6E1mnN+O0Lv6tXC4aS7inFqrPnUlfLlj5U+P6reXGc8pjFY4+101jv3teWJxc5ybvJuT9/Be5aGNE3G1c9ST6slwIwmorO7yRwRNVPoiGmip7vRe4pCDaNGnGSVObmsqbbjl8TWseq6lAysxDfpb+7/uUchBCjaPY5vGsPiIqpLLTleFRt2SXGMnfhZ215K5q5F13fxGWrbkyc+t/S8O9fbsOEk1dO6fB8n1REc6dm+/tbA4mOErTzYTPk+8k0qa1tKEnpHS14vw+nE6JpzUkmuDV179TpLtFmkQANYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMS3q2xWqSlgsBHNiZRtVqv8AKw0JK2eb4OpZ6QWvN6WvqbfzdelsuEacJOdWpBzq1ZPxSd2rKPCMVZ21b1d2zf8AhcJCmmqcFFOUpOySvKTvKTtxbZort2xd8a4+xSpx97cpfSSOXk6dvF21KRRRAibTRTkqsNC7Rf54FZVp5fUtWzKV6iXUzLGYayirW4fVE/LGGbQoZWWmZk23cO0r2MaqIqCUTsJK04+pJuRU5WaF6VLq7dB9mO7mFxezqrr0oVFVkozTis0XBfijNeKLs4apr8JdcHsnHbJmo4bPjNnat0HKP2igtPyZSaVSPHwadNeNq7B8XfD16XNThP8Amjlf/FfE2oMOcY3PjKoacrpO1r+fEiALQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAPGcy9rmKz7QxD8qmX+RKP/AKnTZyfv9Xz4uvLzr1H8ZyOXk7jr4+svxi8Soo8SnRVYdFOTIt2cJnrxTdtePNGS7XnlqqL9rS/JKWn0LPuivvlqutys3hxC+100ne8o6+s+XQidtUO2rSp6ctDDqq1ZkVeu3OcX5v6lgrrVlsimsLEViE1rc3YLiv8AqqkOUqLfvjKH9GzeRzn2LYlx2nRXtxnF++En9Ujownx9L8v9AAOjmAAAAAAAAAAAAAAAAAAAAAAAAAAAAeXA9OQN5J3qzfnN/M3/AL89qeEwCcKVsRiU7OnGdow83Umk7P8AdWvpxOetrzzOTXNp/FHLPuO3jnF/FsRV4cpIsqKTKriyPY9fJJS5DaeJzYiEusf+VzzZEM7stXbhe3Lj8i2Ymq+9X6kvmc8by2pmIq/eTfV/UttRntWr4mS5M6MQMlsikyE1rPuyipbamG/Uvndf1OmzjahjJUouUJSjPLFRkm4yi73umtU9OJsncftnrUctHHp16d/zk/vYp20krWqW1/e9ScOHTyc10ACj2TtSjiaMa1CpGpTlwlF/Jrin0eqKw6OQAAAAAAAAAAAAAAAAAAAAAAAAAAKXae0aWHpSq1qkYU4q8pSdkunV8klqzQfaF2r1sSpUcLmo4d3V72q1VazzW/Ljx0Wr5vkXPt827TlXpYeMryoRlKpq8qnUyZItLRyUU3qtM682YRuDunLaWLjSnJ01KE6inKN1KMKkYSVNftSvL00ZPapwxbCYCtVv3VKdSXswhKTS87RTsifVpyScZRcZK8WpJqScXqmnqmdXbqbpYXZ1Jww8LOVs85a1JtcMz8uNktFc0T2u7K7na2IstKqhWX8Ucs//ACUmTnxHTx3d010RQlqQS0foeQepTiyvdmso1oylqlxXnpaxbcY19pjZad4tOmdaEGz6+WUX8US69T72/wC8n8zlJ/ravhR13aT9TxyPMS/EyXc6TpKIEJHRSurm1snLIN292qu0cVHDUZRjKUZSzSvlioRvd2V+nq0VO+vZ9i9nSj3q7ynJJ99TUnTvdrJNteGXDjxvpztnP/5/2Y3jK+I5U6Sp9M1SUZfJU5fE3liKMZxcZpSi1ZqSTi15NPijMOl53lybulvJisBV7yhUyvTPB/l1EuVSPP8AUtV5nSu4+862jhFX7qVOV3GcW045o8cklxj62Zo7fTdPNhMTtSko06CxWWjRjG0O4Uu6VZeV52dlpZu3Ih7Od7/9PxkJOT+xYqyqp8INeFVFrxi9H5xs/ISpvLpQHiPS0gAAAAAAAAAAAAAAAAAAAAAS8RNqDaV2k2lor2T01aXxJhLrxbi0uLTXxQHHe38VVrVHUqtupWlOpNvW7lKyt+7ZWXRG69xNnqlgNjYpNJxrVqc37VPFSrRUX/uqkaR2zGpGp3dSXioLu+DVsjaWnz950h2fKNfBQzR+6r5MVTitFCanCVWnHoq0c6/7luRE6VWdmp+3fY+alRxcY37tunV/RO+Vv+K6/jNsFHtfZ0MRh6lCovBUi4vzV+a6p2fuNym4Y3V240x1PLJ+T/y5TqRlW9WwZ4avUoVVadNvX2o8VJdLNP0l0MUlGzsThluL8uOrtWUKhMm7yXqvqUNORUqfA2xziVX4ktHtWWpAVGJkSrwcOfL+3/0paUbv6mfdm26bx2MjCS+5g1Os+WVP8u/m+Hvfkc878R18c+a3J2P7EeG2XBy/Mrt1pekklBfypP3syjeGlUnhatOi8tWpBwjK18jn4O9tzypuVuhcIxSVkrJf5Y9sdJNTTnbu7YzvTsOm9i18LCKUIYaUKa427uF6b9U4xfuOWsH+CrTu1KNpw15p2ml1s4v/AGzsmpBOLT4NWfp5HHU45cW1ydSpD+Zyin8zK2TjbqPs729LHbMoYidu8lFxqW4Z6cnCT9+W/vMkMC7EZzexaSmuE6yj+nvZP6tmelRIAAAAAAAAAAAAAAAAAAAAAAADmbtt2JLDbUlPjTxC7yLstHdqdPrZ2fpNGxewvHqeB+zuXioTdSC593UUk49Up956Xj0Mk7Tt0/8AUcBOEIx+0Q8dCT45lbNTvyUkretvIwzsPevjhkqqE6crpxcnTnCOq9tJZZJ8MkXxlIjpU5jcYALSwTtQ3K+3Ue9pJLFUl4XpecePdt+fG3q1zOb8Xg7TcZLLJXTT0aa0tqdlmt+0zs3jjU6+HSjieMo8FU6p8p9eD5+Zyzxs5jt485/OTneGEh3kIXlqvvPDpF3dra6qyi79SS3BRVnJz0vosq6ebZc9o4OcJypVYuFWPh1Ti9LeGSfD3lnu43VtdV14Wf1Nwy9onPD1qbOnDM/E8rtl0WbV65lfR8fkeUKMZKT10a5cmpX+i+PQkQV5K3H/ADUyLd/YtbFVY0MPBylLjb5yb5LXi9Dc8tQww9ql7C2PUxGIjRowbnKVkl5+fpzvyWp1BuVuvT2fhY0oWc3Z1Z2/FO30XBL+7KHcDcels2lynXkvHO2i55Ic1H6/BLLjMMb3W55y8QAB0ckvEVFGEpPhFNv0Suc8737qPA7GoVZxtisTi41anN04qlWlGin0vd9fRHQ9WCkmnwfHqvJmue27BTq4GKhFvK5zemiypLV8m82VeblYnL7VjfhX9jGFybHpyzSfezqz15feOCS6Wgn7zOi07qbKeEwOHw7d5UqUIyfBOSXifxuXY2JAAaAAAAAAAAAAAAAAAAAAAAAAUuH2fShOc4QSlUlmm0vxSyqOd9bJK54AKsAAAABje9m5OE2hH76DVRLw1YWVRdG7WkujRqTeDsPxSu8NXpVV5STpzty11i38D0E+s3tXvdae7sdh+IlJSxtaNKPs07TqPpmfhj8zcu7e7eGwNLu8NTUV+1J6zk/OUnq/TguQAk+S5XWl4ABSQAACGpTUlZpNacdeGqPABGAAAAAAAAAAAAA//9k=" alt="黃緣龍蝨">
            </div>
            <div class="card-body">
                <h3>黃緣龍蝨</h3>
                <p class="card-text">大型水生甲蟲，游泳能力強，是水域生態的重要指標。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-4')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="大刀螳">
            </div>
            <div class="card-body">
                <h3>大刀螳</h3>
                <p class="card-text">大型肉食性昆蟲，前足特化成鐮刀狀捕捉足。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-5')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="善變蜻蜓">
            </div>
            <div class="card-body">
                <h3>善變蜻蜓</h3>
                <p class="card-text">俗稱紅蜻蜓，是龍潭大池周邊常見的蜻蜓種類。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-6')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

    </div>
</section>

        <section id="eco-water" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：水質觀測指標 (張佳琪)</h2>
                <p>監測龍潭大池水體健康的科學核心指標與國家地面水體分類</p>
            </div>
            <div class="water-grid">
                <div class="water-card">
                    <h3>物理與生物指標</h3>
                    <ul class="water-list">
                        <li><strong>水溫 (°C)：</strong> 影響水中溶氧量及水生生物的代謝速度。</li>
                        <li><strong>濁度 (NTU)：</strong> 水中懸浮微粒的多寡，反映水體混濁程度。</li>
                        <li><strong>大腸桿菌群 (CFU/100mL)：</strong> 常作為判定水質是否受到「糞便污染」的衛生指標。</li>
                    </ul>
                </div>
                <div class="water-card">
                    <h3>化學指標</h3>
                    <ul class="water-list">
                        <li><strong>酸鹼值 (pH)：</strong> 天然水體一般在 6.5 ~ 8.5 之間，過酸過鹼皆有害。</li>
                        <li><strong>溶氧量 (DO)：</strong> 水中溶解氧氣量，低於 2 mg/L 會導致水體發臭。</li>
                        <li><strong>生化需氧量 (BOD)：</strong> 微生物分解有機物需氧量，數值越高代表有機污染越重。</li>
                        <li><strong>化學需氧量 (COD)：</strong> 用化學氧化劑分解污染物的需氧量，反映工業廢水污染度。</li>
                    </ul>
                </div>
            </div>
            <div class="welcome-box" style="padding: 20px;">
                <h3 style="color:#38bdf8; margin-bottom:15px;"><i class="fa-solid fa-table"></i> 臺灣地面水體分類及適用用途</h3>
                <table class="struc-table">
                    <thead>
                        <tr>
                            <th style="width: 20%;">水體類別</th>
                            <th style="width: 80%;">適用用途 / 說明</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td style="color:#38bdf8; font-weight:bold;">甲類</td><td>一級公共用水（需經一般標準淨化即可飲用）、生態保育。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">乙類</td><td>二級公共用水（需經特定程序淨化）、一級水產用水（適合高經濟魚類養殖）。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">丙類</td><td>三級公共用水、二級水產用水、一級工業用水（製程用水）。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">丁類</td><td>灌溉用水、二級工業用水（冷卻用水）、環境保育。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">戊類</td><td>環境保育（僅能維持最基本的環境景觀，不發臭）。</td></tr>
                    </tbody>
                </table>
            </div>
        </section>

           <main id="hist-origin" class="page-section">

    <div class="welcome-box text-card">

        <span class="author">撰寫者：劉亞蓁</span>

        <h2>歷史與文化：起源故事</h2>

        <div class="origin-photo">
            <img src="https://scontent.fkhh1-1.fna.fbcdn.net/v/t39.30808-6/505988188_10016236228413046_7521403908418223622_n.jpg?stp=dst-jpg_tt6&cstp=mx720x480&ctp=s720x480&_nc_cat=102&ccb=1-7&_nc_sid=cf85f3&_nc_ohc=v0A_Ee_H-XQQ7kNvwEn0gY3&_nc_oc=Adqo15mqzv3zZi19p2QpoF4fy4Y4rSYR0qQ7lhDzEtWGE2QTv6lYfpOXAJEuw0TwihQ&_nc_zt=23&_nc_ht=scontent.fkhh1-1.fna&_nc_gid=vSOQsEbi4Fvcu9KqJVXxXQ&_nc_ss=7b289&oh=00_Af8ChsBb1POgaR3XAdJAHc7-VdfT8eZYfelgb3vPSm4oig&oe=6A352994" class="origin-img" alt="龍潭大池起源">

            <div class="img-caption">
                龍潭大池早期景觀照片（資料來源：FB【龍潭人】社團）
            </div>
        </div>

        <div class="content-block">

            <h3>一、歷史背景</h3>

            <p class="article-p">
                龍潭大池的歷史可追溯至清代開墾時期。桃園臺地地勢平坦卻缺乏大型河川，降雨後雨水容易迅速流失，因此早期客家先民為解決農業灌溉問題，利用天然低窪地築堤蓄水，逐漸形成今日龍潭大池的前身。
            </p>

        </div>

        <div class="content-block">

            <h3>二、地名演變</h3>

            <p class="article-p">
                早期大池周圍長滿菱角，因此被稱為「菱潭陂」。由於池面寬廣、水色清澈，加上地方流傳池中有靈異神蹟，因此後來又出現「靈潭陂」的稱呼，清代地方志與相關文獻中皆可見類似記載。
            </p>

        </div>

        <div class="content-block">

            <h3>三、龍潭傳說</h3>

            <p class="article-p">
                相傳有居民在池中看見龍形倒影或雲霧盤旋於水面，因此逐漸將「靈潭」轉稱為「龍潭」。這類傳說雖帶有民間信仰色彩，但也反映當地居民對水資源的敬畏與依賴，隨著聚落發展，「龍潭」最終成為地方正式名稱。
            </p>
<div class="origin-photo">
    <img src="data:image/jpeg;base64,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" class="origin-img" alt="龍潭大池傳說">

            <div class="img-caption">
                龍潭大池傳說（資料來源：ubn部落格）
            </div>
</div>
        </div>

    </div>

</main>


       <main id="hist-life" class="page-section">

    <div class="welcome-box text-card">

        <span class="author">撰寫者：劉亞蓁</span>

        <h2>歷史與文化：周邊生活</h2>

        <div class="origin-photo">
           
            
        </div>

        <div class="content-block">

            <h3>一、農業灌溉與居民生活</h3>

            <p class="article-p">
                龍潭大池不僅是灌溉設施，更深刻影響居民的日常生活。早期居民依靠池水灌溉水稻、茶園與旱作農地，因此埤塘與農村經濟密不可分。每逢旱季，地方居民還需依照輪灌制度共同管理水資源，展現早期社區合作精神。
            </p>
<div class="origin-photo">
<img src="data:image/jpeg;base64,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" class="origin-img" alt="龍潭大池農田關係">
 <div class="img-caption">
                龍潭大池農業關係（資料來源：HouseWsb）
            </div>
</div>
        </div>
        
        <div class="content-block">

            <h3>二、聚落形成與公共空間</h3>

            <p class="article-p">
                客家聚落大多沿著埤塘周邊形成，居民除了耕作之外，也利用池塘進行洗滌、捕魚及日常取水。龍潭大池因位於交通與聚落中心位置，逐漸成為地方居民交流、聚會與舉辦活動的重要公共空間。
            </p>
<div class="origin-photo">
<img src="data:image/jpeg;base64,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" class="origin-img" alt="龍潭大池周遭客家房屋">
           <div class="img-caption">
                龍潭大池周遭客家房屋（資料來源：桃園市政府觀光導覽-桃園市政府）
</div>
        </div>

        <div class="content-block">

            <h3>三、現代觀光與文化價值</h3>

            <p class="article-p">
                現代化之後，大池周圍逐漸發展為商業與觀光區。環湖步道、龍潭吊橋、南天宮及各式文化活動吸引大量遊客。雖然農業灌溉功能已逐漸減弱，但龍潭大池仍持續扮演地方文化記憶、觀光發展與社區認同的重要角色。
            </p>

<div class="origin-photo">
<img src="data:image/jpeg;base64,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" class="origin-img" alt="龍潭大池步道">
           <div class="img-caption">
                龍潭大池步道（資料來源：桃園市政府觀光導覽-桃園市政府）
</div>
        </div>

    </div>

</main>


       <main id="hist-name" class="page-section">

    <div class="welcome-box text-card">

        <span class="author">撰寫者：劉亞蓁</span>

        <h2>歷史與文化：周邊生活地名考證</h2>

        <div class="origin-photo">
            <img src="https://hch.hakka.gov.tw/hakkexpertmaintain/cultureImage/170913222036765.jpg" class="origin-img" alt="龍潭地名考證">
 <div class="img-caption">
                龍潭大池地名由來（資料來源：客家雲 Hakka Cloud）
            </div>
        </div>

        <div class="content-block">
            <h3>一、「菱潭陂」的由來</h3>

            <p class="article-p">
                龍潭地名的演變反映了地方歷史與文化發展的歷程。根據地方文獻與學者研究，龍潭大池最早被稱為「菱潭陂」，主要是因為池中盛產菱角，形成特殊的水域景觀，也成為早期居民對此地最直接的命名依據。從名稱中可以看出當時居民與自然環境密切互動的生活樣貌。
            </p>
        </div>

        <div class="content-block">
            <h3>二、「靈潭陂」的形成</h3>

            <p class="article-p">
                隨著地方信仰的發展，居民逐漸認為池水具有靈驗力量，因此出現了「靈潭陂」的稱呼。在客家話中，「潭」代表較大的水池或深水區，而「靈」則象徵神聖與神秘，使地名增添宗教信仰與民間傳說色彩。
            </p>
        </div>

        <div class="content-block">
            <h3>三、「龍潭」名稱的確立</h3>

            <p class="article-p">
                清末至日治時期，地方逐漸普遍使用「龍潭」一詞。民間相傳池中曾出現龍形倒影或龍氣升騰，因此以「龍潭」取代原本的「靈潭」，並成為今日行政區正式名稱，龍潭大池也成為地方重要的歷史文化象徵。
            </p>
        </div>

    </div>

</main>


        <section id="water-sys" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：李芷嫻</span>
                <h2>地景與水利：地形與水源環境</h2><br>
                <h3>1. 地形環境</h3>
                <p class="article-p">龍潭大池位於桃園臺地，是典型的<strong>「埤塘地景」</strong>。桃園臺地因河川短、小、流速快，不容易直接取河水灌溉，因此先民利用天然低窪地與人工築堤形成埤塘蓄水。</p>
                <p class="article-p">龍潭大池原名包含：菱潭埤（因長滿菱角）、靈潭埤、龍潭埤，這也是後來成為「龍潭」地方行政區地名的重要來源。</p>
                
                <h3>2. 景觀構成</h3>
                <p class="article-p">整個龍潭大池的環境地景包含了：大面積水域、環湖堤岸、吊橋與忠義橋、南天宮人工島、周邊聚落與農田以及錯綜複雜的水圳系統。其中，<strong>「池塘 ＋ 聚落 ＋ 農田」</strong>的三位一體結構，正是桃園埤塘文化最核心的景觀特色。</p>
            </div>
        </section>

        <section id="water-struc" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：李芷嫻</span>
                <h2>地景與水利：關鍵水利構造</h2><br>
                <p class="article-p">龍潭大池並非純天然湖泊，而是透過「天然窪地 ＋ 人工引水 ＋ 築堤蓄水」所形成的永續灌溉系統。其上游水源主要引自老街溪上游、風櫃口埤，再經由渠道引入池中。以下為系統內六大關鍵水利建構細節：</p>
                
                <table class="struc-table">
                    <thead>
                        <tr>
                            <th style="width: 25%;">關鍵構造</th>
                            <th style="width: 75%;">核心功能與數據規模</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(1) 埤塘（蓄水）</td>
                            <td>負責儲存雨水與引水，於旱季時供應全面灌溉。龍潭大池的有效蓄水量高達約 165,000 立方公尺。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(2) 堤岸</td>
                            <td>主要用以防止池水外流並穩定大池水位，其大池堰堤全長約有 1200 公尺。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(3) 進水口</td>
                            <td>屬於「集水系統」的核心，負責將上游圳道的珍貴水源引導流入大池。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(4) 出水口（水門）</td>
                            <td>負責動態控制放水量並合理分配下游灌溉用水，大池下游設有兩個主要出水口。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(5) 水圳</td>
                            <td>水資源的輸送動脈，負責將池水精準輸送到各區農田。其中著名的龍潭圳全長達 3700 公尺。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(6) 分汴水門</td>
                            <td>如同「水資源交通分流系統」，負責把主水圳的水量分流到不同的支線農地中，全區共設有約 40 處分汴水門。</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

        <section id="water-logic" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：李芷嫻</span>
                <h2>地景與水利：傳統水利分水邏輯</h2><br>
                <p class="article-p">龍潭大池歷久不衰的配水與分水智慧，可以被直觀地簡化為以下的高效線性流程：</p>
                
                <div class="flowchart">
                    <span class="flow-step">上游來水</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">進水口</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">龍潭大池蓄水</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">水門控制放流</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">主水圳</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">分汴水門</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">各農田精準灌溉</span>
                </div>

                <h3 style="margin-top: 20px;">分水系統四大核心運作原理：</h3>
                <ul style="padding-left: 20px; margin-top: 10px;">
                    <li style="margin-bottom: 10px;"><strong>1. 先蓄後放：</strong>於豐水雨季提前存蓄水源，在枯水旱季時再穩定釋放供應。</li>
                    <li style="margin-bottom: 10px;"><strong>2. 高處到低處（重力流）：</strong>完美利用地理自然地勢的高低落差，使水流依重力自然向下流動，無需耗費大量電力與抽水機，兼顧低碳環保。</li>
                    <li style="margin-bottom: 10px;"><strong>3. 分區輪灌機制：</strong>各區農田不重疊搶水，採取依序、排程放水，徹底解決上游搶水、下游無水可用的分配不均問題。</li>
                    <li style="margin-bottom: 10px;"><strong>4. 水門流量精密控制：</strong>因應各農地大小，靠調整水門開關大小與水位高度決定分流量，體現古代與現代交織的水利環境工程智慧。</li>
                </ul>
            </div>
        </section>

       
      <section id="now-land"class="page-section">
    <div class="welcome-box text-card">
        <span class="author">撰寫者：劉亞蓁</span>
        <h2>變遷與現狀：土地變化</h2><br>
        
        <div class="content-block">
            <h3>一、清代至日治時期：典型埤塘農業景觀</h3>
            <p class="article-p">
                龍潭大池周邊土地利用型態歷經明顯變遷。清代至日治時期，大池主要服務農業灌溉需求，周圍多為水田、茶園及客家聚落，形成典型的埤塘農業景觀。
            </p>
            <div class="origin-photo">
            <img src="data:image/jpeg;base64,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" class="origin-img" alt="台灣環境資訊協會">
 <div class="img-caption">
                龍潭大池早期水田（資料來源：台灣資訊協會）
            </div> 
        </div>

        <div class="content-block">
            <h3>二、戰後時期：都市發展與空間縮減</h3>
            <p class="article-p">
                戰後人口增加與都市發展加速，部分農地逐漸轉變為住宅區、商業區及交通設施。原本廣大的農業空間逐漸縮減，大池的灌溉功能也相對降低。
            </p>
             
        <h3>三、近年現狀：現代化生態與觀光轉型</h3>
            <p class="article-p">
                近年政府推動環境整治與景觀改善工程，興建環湖步道、觀景平台與生態設施，使龍潭大池由傳統農業埤塘轉型為兼具生態保育、觀光休閒及環境教育功能的重要公共空間。
            </p>
            <div class="origin-photo">
            <img src="data:image/jpeg;base64,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" class="origin-img" alt="自由時報">
 <div class="img-caption">
                龍潭大池環湖步道（資料來源：自由時報）
            </div>
           </div>
        </div>
    </div>
</section>

<section id="now-func" class="page-section">
    <div class="welcome-box text-card">
        <span class="author">撰寫者：劉亞蓁</span>
        <h2>變遷與現狀：轉型功能</h2>
        
        <div class="content-block">
            <h3>一、核心轉型：從單一灌溉到多元空間</h3>
            <p class="article-p">
                現代龍潭大池結合了觀光、滯洪、多層複合濾料（MSL）及礫間水質淨化科技，成功由傳統的單一農業灌溉功能，轉型為兼具多功能與環保思維的多元綠色環境教育場所。
            </p>
        </div>

        <div class="content-block">
            <h3>二、現代三大功能解構</h3>
            <ul class="function-list" style="list-style: none; padding-left: 0;">
                <li style="margin-bottom: 15px;">
                    <strong>• 觀光休閒：</strong>興建完善的環湖步道與水岸景觀，成為兼具美學與遊憩價值的都會親水空間。
                </li>
                 <div class="origin-photo">
            
            <img src="data:image/jpeg;base64,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" class="origin-img" alt="龍潭水岸景觀">
 <div class="img-caption">
                龍潭大池水岸景觀（資料來源：桃園市政府）
            </div>
 
                <li style="margin-bottom: 15px;">
                    <strong>• 城市滯洪：</strong>在面對極端氣候與豪大雨時，大池承擔起調節周邊區域水量的滯洪重任，保障周邊居民的安全。
                </li>
                <li style="margin-bottom: 15px;">
                    <strong>• 水質淨化科技：</strong>導入先進的「多層複合濾料（MSL）系統」與「礫間水質淨化設施」，利用純自然、低碳的生物濾料與微生物分解，有效淨化流入大池的水質。
                </li>
            </ul>
        </div>
    </div>
</section>

<section id="now-issue" class="page-section">
    <div class="welcome-box text-card">
        <span class="author">撰寫者：劉亞蓁</span>
        <h2>變遷與現狀：面臨挑戰</h2>
        
        <div class="content-block">
            <h3>一、水質問題：水體優養化的危機</h3>
            <p class="article-p">
                雖然龍潭大池已完成多項環境改善工程，但目前仍面臨水質優養化的嚴峻考驗。當周邊的民生污水或農業流出的氮、磷等營養鹽過量進入水體時，容易引發藻類在短時間內大量繁殖，進而導致水中的溶氧量急遽下降，嚴重威脅底棲生物與水域生態的健康。
            </p>
        </div>

        <div class="content-block">
            <h3>二、生態衝擊：外來入侵種的威脅</h3>
            <p class="article-p">
                外來入侵種是維持大池生態平衡的另一個重要挑戰。例如布袋蓮與克氏原螯蝦（美國螯蝦）等外來物種，由於缺乏天敵且適應力極強，在水域中快速擴散繁殖，不僅壓縮了原生物種的生存空間，也逐漸破壞了埤塘既有的複雜生態網絡。
            </p>
        </div>

        <div class="content-block">
            <h3>三、環境治理：人為干擾與氣候考驗</h3>
            <p class="article-p">
                此外，高度都市開發帶來的人為干擾、日益增加的觀光人潮壓力，以及全球氣候變遷所引發的極端豪雨與乾旱現象，都對大池的水資源管理與調蓄功能形成全新的考驗。未來如何在觀光發展、生態保育與在地文化保存之間取得平衡，將是龍潭大池走向永續經營最核心的課題。
            </p>
        </div>
    </div>
</section>

        <section id="task-choice" class="page-section">
            <div class="section-title-box">
                <h2>互動任務：環境教育知識測驗</h2>
                <p>請點擊下方選項進行即時檢測，系統將自動給予評分與解析。</p>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q1. 龍潭大池的主要水源主要來自下列哪一個系統？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-origin', '大圳與老街溪系統')">A. 自來水加壓供水系統</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-origin', '大圳與老街溪系統')">B. 工業廢水循環系統</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-origin', '大圳與老街溪系統')">C. 大圳與老街溪引水系統</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-origin', '大圳與老街溪系統')">D. 深層地下溫泉系統</button>
                <div id="ans-origin" class="analysis-box"><strong>標準答案：C</strong><br>解析：根據地景水利調查文本，大池主要依靠渠道引入老街溪上游及風櫃口埤之水源，而非自來水、工業水或溫泉。</div>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q2. 龍潭大池最早在先民開發、闢建時，最主要的初衷與核心目的為何？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-1', 'B')">A. 水力發電與工業供水</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-1', 'B')">B. 蓄水灌溉農田周邊</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-1', 'B')">C. 開闢海運與商務貿易港口</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-1', 'B')">D. 專門用於大型高密度捕魚與水產養殖</button>
                <div id="ans-1" class="analysis-box"><strong>標準答案：B</strong><br>解析：先民因應桃園臺地蓄水不易，利用低窪地築堤，最主要的原始核心功能即為農業灌溉。</div>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q3. 負責在主水圳上將珍貴的水資源流向各個不同區塊農地的關鍵「交通分流構造」是什麼？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-2', 'D')">A. 生態曝氣池</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-2', 'D')">B. 環池阻水堰堤</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-2', 'D')">C. 大型發電抽水機</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-2', 'D')">D. 分汴水門</button>
                <div id="ans-2" class="analysis-box"><strong>標準答案：D</strong><br>解析：全區擁有大約 40 處的「分汴水門」，其功能正是如同水流的紅綠燈，將主圳的水流合理分撥至不同農地。</div>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q4. 龍潭大池下游的水圳在輸送水資源時，主要是依循什麼科學原理來達成永續節能？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-3', 'A')">A. 太陽能光電加壓幫浦</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-3', 'A')">B. 利用地形自然地勢的重力高低差</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-3', 'A')">C. 大規模機械潮汐逆流泵</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-3', 'A')">D. 風力虹吸虹吸效應</button>
                <div id="ans-3" class="analysis-box"><strong>標準答案：B</strong><br>解析：大池設計完美順應地形，讓水自然從高處流向低處，因此不需要抽水機就能自然灌溉。</div>
            </div>
        </section>

        <section id="task-fill" class="page-section">
            <div class="section-title-box">
                <h2>互動任務：填空檢測與簡答思維</h2>
                <p>請在填空欄中鍵入正確名詞並點擊檢查；簡答思考題可點擊下方按鈕對照參考答案。</p>
            </div>

            <div class="welcome-box">
                <h3 style="color:#38bdf8; margin-bottom:15px;"><i class="fa-solid fa-pen-to-square"></i> 水利構造與脈絡填空題</h3>
                <ol style="padding-left: 20px; line-height: 2;">
                    <li>龍潭大池位居桃園臺地，屬於台灣非常具有在地特色的典型「 <input type="text" id="bk-1" class="fill-blank-input" placeholder="兩個字"> 」地景。</li>
                    <li>大池在調節水資源時，主要是運用智慧採取「先 <input type="text" id="bk-2" class="fill-blank-input" style="width:50px;" placeholder="字"> 後 <input type="text" id="bk-3" class="fill-blank-input" style="width:50px;" placeholder="字"> 」的邏輯彈性供水。</li>
                    <li>在水利系統中，具備動態調控大池放水量、重新分配重要水流的渠道卡閘設施，我們稱之為「 <input type="text" id="bk-4" class="fill-blank-input" placeholder="兩個字"> 」。</li>
                    <li>長度達 3700 公尺的龍潭圳，在整體架構中肩負的核心功能，是將大池水精準送到下游的「 <input type="text" id="bk-5" class="fill-blank-input" placeholder="兩個字"> 」中。</li>
                </ol>
                <button class="check-btn" onclick="validateBlanks()">提交檢查填空答案</button>
                <span id="bk-result" class="result-text"></span>
            </div>

            <div class="welcome-box">
                <h3 style="color:#38bdf8; margin-bottom:15px;"><i class="fa-solid fa-comments"></i> 環境與人文深度簡答思考</h3>
                
                <div style="margin-bottom: 20px;">
                    <p style="font-weight:bold; color:#cbd5e1;">(1) 假設早期在沒有埤塘系統的輔助下，桃園臺地的農業發展將會遭遇什麼致命性的困境？</p>
                    <textarea id="box-1" class="text-area-answer" placeholder="請在此輸入您的見解與回答..."></textarea>
                    <button class="check-btn" onclick="toggleRef('reply-1')">顯示 / 隱藏 參考解答</button>
                    <div id="reply-1" class="reference-answer">
                        <strong>環境學科參考觀點：</strong> 農業將面臨毀滅性的常態缺水。由於桃園臺地地勢特殊，河川太短且流速過快，降雨很快就會流入大海、無法蓄留。少了埤塘在雨季進行「先蓄後放」，到旱季時農田將完全乾涸。
                    </div>
                </div>

                <div style="margin-bottom: 20px;">
                    <p style="font-weight:bold; color:#cbd5e1;">(2) 在全面高度都市化的現代社會中，你認為我們還需要完整保留傳統埤塘嗎？理由為何？</p>
                    <textarea id="box-2" class="text-area-answer" placeholder="請在此輸入您的見解與回答..."></textarea>
                    <button class="check-btn" onclick="toggleRef('reply-2')">顯示 / 隱藏 參考解答</button>
                    <div id="reply-2" class="reference-answer">
                        <strong>環境學科參考觀點：</strong> 依然極度需要。現代埤塘已由單純灌溉成功轉型為多元韌性功能，例如：在暴雨時充當「都市防洪滯洪池」調節洪峰、減緩熱島效應以調節都市微氣候，更是珍貴的都市綠帶、生態保育及環境教育的活教材。
                    </div>
                </div>

                <div>
                    <p style="font-weight:bold; color:#cbd5e1;">(3) 現代的龍潭大池除了延續部分傳統水利功能，還延伸被賦予了哪些當代的永續新使命？</p>
                    <textarea id="box-3" class="text-area-answer" placeholder="請在此輸入您的見解與回答..."></textarea>
                    <button class="check-btn" onclick="toggleRef('reply-3')">顯示 / 隱藏 參考解答</button>
                    <div id="reply-3" class="reference-answer">
                        <strong>環境學科參考觀點：</strong> 現代大池不僅是著名的觀光休閒與文化地標（南天宮、吊橋），更導入現代環保科技（如多層複合濾料MSL、生態礫間曝氣）進行主動式水質再淨化，成為實踐環境友善與水環境教育的核心基地。
                    </div>
                </div>

            </div>
        </section>

    </main>

    <div id="customModal" class="custom-modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal()">&times;</span>
            <h2 id="modalTitle">標題</h2>
            <hr>
            <p id="modalBody">詳細紀錄內容載入中...</p>
        </div>
    </div>

    <footer class="main-footer">
        <p>&copy; 2026 龍潭大池埤塘數位資料庫. 專題環境教育平台. All Rights Reserved.</p>
    </footer>

    <script>
        // 1. 單頁式網頁分頁切換控制 (SPA 邏輯)
        function showPage(sectionId) {
            // 隱藏所有分頁
            const sections = document.querySelectorAll('.page-section');
            sections.forEach(sec => sec.classList.remove('active'));
            
            // 秀出對應的分頁
            const targetSection = document.getElementById(sectionId);
            if (targetSection) {
                targetSection.classList.add('active');
                // 換頁時自動置頂
                window.scrollTo({ top: 0, behavior: 'smooth' });
            }
        }

        // 2. 原生態卡片 Modal 互動控制
        function openModal(type) {
            const modal = document.getElementById('customModal');
            const title = document.getElementById('modalTitle');
            const body = document.getElementById('modalBody');
           
            
            // 植物、動物與鳥類詳情文本映射庫
            const infoData = {
            'modal-plant-1': {
    t: '台灣萍蓬草 (水蓮花)',
    b: `🌿 <strong>台灣萍蓬草 (Nuphar shimadae Hayata)</strong><br><br>

<strong>一、基本資料與分類</strong><br>
台灣萍蓬草（Nuphar shimadae Hayata）是睡蓮科萍蓬草屬植物，也是台灣特有的珍貴水生植物。其種小名 shimadae 是為了紀念日治時期植物採集家島田彌市，由植物學家早田文藏於1916年正式發表命名。<br><br>

台灣萍蓬草俗稱「水蓮花」，過去廣泛分布於桃園、新竹等地的埤塘，是台灣水域生態的重要代表物種。由於它是全球萍蓬草屬中分布最南端的種類，因此具有相當高的植物研究與保育價值。<br><br>

<strong>二、形態特徵與外觀辨識</strong><br>
台灣萍蓬草為多年生浮葉性水生植物，為適應水域環境演化出特殊構造。<br><br>

• 地下莖：粗大橫臥於水底泥土中，外形類似生薑，表面留有葉柄脫落的痕跡。<br>
• 浮葉：漂浮於水面，呈倒卵形或愛心狀，翠綠光滑，可隨水位變化伸長葉柄。<br>
• 沉水葉：生長於水中，質地較薄且帶有皺褶，有助增加光合作用效率。<br>
• 花朵：全年皆可開花，特色為「黃花紅心」，金黃色花萼搭配鮮紅色放射狀柱頭，辨識度極高。<br><br>

`
},
                'modal-plant-2': {
    t: '香蒲 (水蠟燭)',
    b: `🌾 <strong>香蒲 (Typha orientalis Presl)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
香蒲（學名：Typha orientalis Presl）是香蒲科香蒲屬的多年生挺水性水生植物，也是台灣的原生種。它廣泛分佈於台灣全島的低海拔埤塘、沼澤、濕地、荒廢稻田或流速緩慢的溝渠。<br><br>

在民間，香蒲擁有許多親切的俗名，最常見的就是「水蠟燭」，另外也被稱為東方香蒲、蒲草或蒲黃。<br><br>

<strong>二、形態特徵與獨特的花序構造</strong><br>
香蒲的外觀非常具有辨識度，尤其是它那長得像熱狗香腸的花序，是水邊最亮眼的景觀：<br><br>

• 地下莖與葉片：具有粗壯的橫走地下根莖，能深深扎根於水底泥土並向外蔓延。葉片呈線形、長條狀，直立挺拔，內含海綿質氣室，長度可達 1 到 2 公尺。<br>
• 神奇的蠟燭花序：這根「水蠟燭」其實是由成千上萬朵極小的花組成的穗狀花序，屬於雌雄同株。<br>
• 雄花序在上方：形狀較為纖細，成熟時會佈滿金黃色的花粉。<br>
• 雌花序在下方：形狀較為肥大，也就是我們肉眼看到、形似褐色香腸的部位。<br>
• 品種辨識技巧：在龍潭大池觀察時，可以看到台灣原生「東方香蒲」的上方雄花序與下方雌花序是緊密相連、沒有間隔的；這點與外來種「狹葉香蒲」有明顯的區別。<br><br>


`
},
               'modal-plant-3': {
    t: '風車草 (輪傘草)',
    b: `🌿 <strong>風車草 (Cyperus alternifolius L.)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
風車草（學名：Cyperus alternifolius L.）屬於莎草科莎草屬，是多年生常綠挺水草本植物。它原產於非洲馬達加斯加島，引進台灣後因適應力極強，目前已廣泛歸化於台灣全島的濕地、河岸與埤塘。在龍潭大池等埤塘生態工程中，它是非常重要的護岸與景觀主力植栽。<br><br>

在民間與園藝界，它因為獨特的外型而擁有許多形象生動的俗名，像是輪傘草、輪傘莎草或繖楊草。<br><br>

<strong>二、形態特徵與獨特的外觀結構</strong><br>
風車草的外觀極具幾何美感與觀賞價值，其主要的植物構造特徵如下：<br><br>

• 莖與高度：具有發達的地下莖，會叢生擴展。它的稈（莖）直立挺拔，形狀呈現獨的三稜形，表面光滑且質地堅硬，高度通常可以達到 50 到 150 公分。<br>
• 獨特的輪傘狀放射葉：在直立莖的頂端，長有 20 到 40 片線形的放射狀苞片。這些苞片扁平且長，向四周水平展開，看起來就像是一把撐開的雨傘骨架，也像一具綠色的紙風車。我們肉眼看到的這圈綠色風車構造，在植物學上其實是「苞片」（變態葉），而它真正的葉片已經退化成包裹在莖基部的鞘狀物。<br>
• 花朵與果實：它的花序就長在頂端綠色風車的中央。花期主要在夏秋兩季，會開出許多密集的綠褐色小穗狀花序。雖然花朵並不鮮豔，但特殊的幾何排列方式讓它整體顯得非常精緻。<br><br>

`
},
                'modal-plant-4': {
    t: '蓮 (荷花)',
    b: `🪷 <strong>蓮 (Nelumbo nucifera Gaertn.)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
蓮（學名：Nelumbo nucifera Gaertn.）是蓮科蓮屬的多年生大型挺水性水生植物，廣泛分佈於亞洲溫帶與熱帶地區。在台灣，它是非常傳統且重要的經濟與景觀水生植物。<br><br>

在古典文學與民間，它擁有荷花、蓮花、菡萏、芙蓉等優美的別稱，其地下莖則俗稱為「藕」。在龍潭大池等埤塘中，蓮花不僅是夏日的視覺焦點，也是維持水域微生態平衡的關鍵角色。<br><br>

<strong>二、形態特徵與獨特的物理特性</strong><br>

• 根莖與葉片：根莖（蓮藕）肥大且多節，橫生於水底的污泥之中，內部有許多縱向的空腔（氣道），能將水面上的空氣輸送到地下根部。葉片從基部抽生，具有長花梗與葉柄，通常高高挺出水面，形狀呈圓盾形，直徑可達 30 到 90 公分。<br>
• 神奇的奈米結構（荷葉效應）：如果你觀察龍潭大池的蓮葉，會發現當雨水落到葉面上時，不會擴散開來，而是會形成一顆顆晶瑩剔透的水珠滾落。這種「出淤泥而不染」的現象在科學上被稱為「荷葉效應」。這是因為蓮葉表面密布奈米級的微小乳突，並覆蓋有一層蠟質結晶，當水珠滾落時，會順便帶走葉面上的灰塵與污垢，達到自我清潔的效果。<br>
• 花朵與果實：花期主要在夏季，花梗比葉柄更高，單生於頂端。花瓣大而美麗，顏色有粉紅色、白色等。花托在開花時呈倒圓錐形，俗稱蓮蓬。<br><br>

`
},
                'modal-plant-5': {
    t: '鴨舌草',
    b: `🪻 <strong>鴨舌草 (Monochoria vaginalis (Burm. f.) Presl)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
鴨舌草（學名：Monochoria vaginalis (Burm. f.) Presl）屬於雨久花科雨久花屬，是台灣原生的多年生或一年生草本挺水植物。傳統上廣泛分佈於全台灣低海拔的稻田、水渠、池塘及濕地。<br><br>

在台灣早期農村，它因為用途廣泛而擁有學堂菜、福菜、鴨仔草或少花雨久花等俗名。在龍潭大池的生態池中，它是非常具有台灣早期農村意象的在地指標植物。<br><br>

<strong>二、形態特徵與名稱由來</strong><br>
鴨舌草的外觀小巧玲瓏，它的名字由來與它的葉片形狀有著極大的關係，其主要的形態特徵如下：<br><br>

• 葉片與獨特形狀：這正是它被稱為鴨舌草的原因。它的葉子具有長柄，從基部叢生。葉片的形狀變化較大，通常呈現卵狀披針形或心形，長度約 2 到 7 公分。因為葉片表面光滑翠綠，先端漸尖，整體形狀看起來就像一隻隻鴨子的舌頭，在水邊成片生長時非常逗趣且好辨識。<br>
• 夢幻的藍紫色花朵：鴨舌草的花期主要在夏秋兩季。它的花序屬於總狀花序，通常隱藏在葉柄的基部鞘狀構造中挺出，每個花序會開出 3 到 20 朵小花。花瓣呈現非常優雅、精緻的藍紫色或淡藍色。雖然每朵花的花期很短（通常清晨開放，傍晚就謝了），但因為花朵數量多、陸續綻放，當成片盛開時，會在水岸邊形成一片美麗的藍紫色浪漫花海。<br><br>

`
},
               'modal-plant-6': {
    t: '布袋蓮 (強勢外來種警告)',
    b: `🪻 <strong>布袋蓮 (Eichhornia crassipes (Mart.) Solms)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
布袋蓮（學名：Eichhornia crassipes (Mart.) Solms）屬於雨久花科布袋蓮屬，是多年生漂浮性水生植物。它原產於南美洲亞馬遜河流域，因其花朵極具觀賞價值而被引進世界各地，目前已廣泛歸化於台灣全島的河流、埤塘、溝渠與濕地。<br><br>

在民間它有許多形象生動的別稱，如鳳眼蓮、水葫蘆、浮水蓮花或鳳眼藍。在龍潭大池中，它是兼具強大淨水功能與高密度管理需求的代表性漂浮植物。<br><br>

<strong>二、形態特徵與獨特的物理特性</strong><br>

• 神奇的布袋葉柄（氣室構造）：這正是它被稱為布袋蓮或水葫蘆的原因。它的葉片呈卵圓形，表面光滑且質地堅煙。最特別的是它的葉柄，基部會明顯膨大，外觀看起來就像一個個飽滿的布袋。如果將葉柄切開，會發現內部充滿蜂窩狀的海綿組織，裡面儲存了大量空氣。這種獨特的氣室構造提供了強大浮力，讓整株植物能安穩地隨波漂流，即使水位暴漲也不會淹死。<br>
• 茂密的羽狀黑根：布袋蓮不扎根於底泥，而是讓根系完全懸浮在水中。它的根系非常發達，呈現羽毛狀分支，顏色多為紫黑色或深褐色。<br>
• 絕美的鳳眼花朵：它的花期主要在夏秋兩季。花莖從葉叢中央抽出，屬於穗狀花序。每朵花有六枚花瓣，呈現優雅的淡藍紫色。最迷人的地方在於最上方的那枚花瓣，正中央有一塊耀眼的黃色斑點，外圍則環繞著深藍色的暈紋，看起來就像是孔雀尾羽上的美麗眼斑，也像一隻神祕的鳳眼，因此它在全球也被廣泛稱為鳳眼蓮。<br><br>

`
},
                'modal-animal-1': {
    t: '原生指標：斑龜',
    b: `🐢 <strong>斑龜 (Mauremys sinensis)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
斑龜（學名：Mauremys sinensis）屬於地龜科擬水龜屬（舊分類為潮龜科），是台灣最常見的原生種淡水龜。牠們廣泛分佈於台灣全島低海拔的河流、湖泊、埤塘、稻田及沼澤濕地。<br><br>

<strong>二、形態特徵與名稱由來</strong><br>

• 獨特的花紋與名稱由來：牠的頭部、頸部和四肢的皮膚上，都密布著黃綠色與深褐色相間的縱向線條。<br>
• 龜殼與年齡特徵：斑龜的背甲通常呈現長橢圓形，顏色為深褐色或黑褐色，背甲上具有三條明顯的縱向稜脊（背線）。<br>
• 長尾巴與幼龜誤稱：在幼龜時期，斑龜的尾巴特別長，長度甚至可以接近背甲的一半，因此常被稱為長尾龜。<br><br>

<strong>三、生態習性：大池畔的「日光浴大師」</strong><br>

• 外溫動物的體溫調節：爬蟲類屬於外溫動物（變溫動物），無法像人類一樣在體內自動調節體溫。牠們必須藉由趴在岸邊、石頭或浮木上「曬太陽」（烘甲），利用太陽的熱能來提升體溫，進而促進身體的新陳代謝與腸胃消化。<br>
• 殺菌與甲殼健康：曬太陽還能幫助斑龜利用紫外線（UV）殺滅背甲上附著的寄生蟲、真菌和藻類，防止龜殼爛甲。同時，紫外線還能幫助牠們體內合成維生素 D3，促進鈣質吸收，讓龜殼生長得更堅固健康。<br><br>

`
},
               'modal-animal-2': {
    t: '蓬萊草蜥',
    b: `🦎 <strong>蓬萊草蜥 (Takydromus stejnegeri)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
蓬萊草蜥（學名：Takydromus stejnegeri）屬於正蜥科草蜥屬，是台灣特有種野生蜥蜴。牠們廣泛分佈於台灣全島低海拔的草生地、灌木叢、林緣以及農田埤塘周邊。<br><br>

<strong>二、形態特徵與名稱由來</strong><br>
• 體色與保護色：身體主要以翠綠色、黃綠色或褐色為主。牠的身體背部多為褐色或深褐色，身體兩側則有一條明顯的鮮黃色或淺綠色縱帶，從眼睛下方一直延伸到尾巴基部。<br>
• 驚人的長尾巴：牠的尾巴極長，長度通常可以達到身體長度（吻肛長）的兩倍以上。<br>
• 鼠蹊孔辨識法：在生物學辨識上，蓬萊草蜥最關鍵的特徵是牠具有「兩對鼠蹊孔」（位於後肢股部內側的腺體構造），<br><br>

<strong>三、生態習性：草叢裡的「日光浴常客」與獨特斷尾術</strong><br>

• 體體溫調節：與斑龜一樣，蓬萊草蜥也是外溫動物。清晨時分，牠們會爬到龍潭大池畔較高的芒草梢或灌木頂端，身體扁平地貼在葉片上曬太陽，藉此快速提升體溫。<br>
• 神奇的求生斷尾術：當蓬萊草蜥遇到伯勞鳥、蛇類或野貓等天敵襲擊時，牠們會啟動一種稱為「自割」的求生機制——主動將尾巴斷掉。<br><br>

`
},
               'modal-animal-3': {
    t: '貢德氏赤蛙 (狗蛙)',
    b: `🐸 <strong>貢德氏赤蛙 (Hylarana guentheri)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
貢德氏赤蛙（學名：Hylarana guentheri）屬於赤蛙科水蛙屬（舊分類為赤蛙屬），是台灣原生種的淡水兩棲類動物。牠們廣泛分佈於台灣全島低海拔的平地、稻田、溝渠、埤塘及草澤環境。<br><br>

<strong>二、形態特徵與獨特的花紋辨識</strong><br>

• 經典的背側褶構造：要辨識貢德氏赤蛙，最明顯的特徵就是牠們身體兩側那兩條極為顯眼的「背側褶」。<br>
• 鼓膜與眼部特徵：牠們的眼睛後方有一個非常大且明顯的圓形結構，稱為「鼓膜」，其大小幾乎跟牠們的眼睛差不多大。<br>
• 體色與保護色：牠們的背部皮膚相當光滑，顏色多為黃褐色、淺褐色或紅褐色，腹部則為乾淨的白色或淡黃色。當牠們靜止在龍潭大池畔的枯草堆或泥岸邊時，這種樸素的褐色調是極佳的保護色。<br><br>

<strong>三、生態習性：大池裡震撼人心的「狗叫聲」</strong><br>
每到春夏兩季的夜晚（約 5 月到 8 月）>：<br><br

• 獨一無二的求偶鳴叫：雄性的貢德氏赤蛙具有一對外咽囊。每到繁殖期，牠們會趴在水草叢或浮葉植物上，發出如同小狗汪汪叫的聲音，這也是牠們狗蛙俗名的由來。<br>
• 耐熱與耐汙染的強韌生命力：相較於其他對環境極度挑剔的保育類青蛙，貢德氏赤蛙對環境的適應力非常強。<br><br>

`
},
                'modal-animal-4': {
    t: '生態大魔王：克氏原螯蝦',
    b: `🦞 <strong>克氏原螯蝦 (Procambarus clarkii)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
克氏原螯蝦（學名：Procambarus clarkii）屬於螯蝦科原螯蝦屬，是原產於北美洲中南部（主要是美國東南部與墨西哥北部）的淡水蝦類。因為觀賞、食用等經濟目的被人類引進台灣，隨後逃逸或遭棄養，目前已在台灣全島廣泛歸化。<br><br>

<strong>二、形態特徵與強大的防禦構造</strong><br>
克氏原螯蝦的外觀就像是一隻縮小版的海洋龍蝦，全身充滿了戰鬥型的防禦與攻擊武器，其主要形態特徵如下：<br><br>

• 招牌的鮮紅大螯：成蝦的體長通常在 7 到 12 公分之間。最顯眼的特徵就是牠們那一對巨大且強壯的「第一步足」（大螯）。成年螯蝦的身體與大螯通常會呈現鮮豔的暗紅色或橘紅色，大螯表面還密布著許多凸起的小刺。<br>
• 堅硬的外骨骼與多層次防護：牠們擁有非常堅硬的甲殼（頭胸甲與腹甲），能抵擋水鳥或中小型魚類的零星攻擊。頭胸甲的前端延伸出一個尖銳的額角，能保護頭部。腹部（俗稱蝦尾）肌肉發達，在遇到危險時，牠們會利用腹部快速向後彈跳，瞬間逃離戰場。<br><br>

<strong>三、生態習性：無所不吃與打洞破壞王</strong><br>

• 極致的雜食性與耐受力：牠們是徹頭徹尾的機會主義捕食者。舉凡水生植物的嫩芽根莖、藻類、死魚屍體、水生昆蟲、小魚、小蝦、蝌蚪，甚至是青蛙的卵塊，只要抓得到、塞得進嘴巴裡，牠們無所不吃。<br>
• 埤塘殺手——掘穴習性：克氏原螯蝦非常喜歡在水岸邊的泥質土堤上挖掘深洞作為巢穴，洞穴深度有時可達數十公分。這種「打洞」的行為對傳統埤塘來說是巨大的災難。大量螯蝦在龍潭大池的岸邊泥土中日夜挖掘，會導致土堤結構變得鬆散破碎。<br><br>
`
},
                'modal-animal-5': {
    t: '化妝大師：黑眶蟾蜍',
    b: `🐸 <strong>黑眶蟾蜍 (Duttaphrynus melanostictus)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
黑眶蟾蜍（學名：Duttaphrynus melanostictus）屬於蟾蜍科亞洲蟾蜍屬，是台灣原生種的兩棲動物。牠們廣泛分佈於台灣全島低海拔的平地、農田、果園、城鎮社區以及埤塘周邊。<br><br>

<strong>二、形態特徵與名稱由來：自帶濃黑眼線的化妝大師</strong><br>
黑眶蟾蜍的體型屬於中大型，成蛙體長大約在 5 到 10 公分之間。：<br><br>

• 招牌的黑眶與黑色指甲：仔細觀察牠們的頭部，從吻端、眼睛上方一直到鼓膜周圍，長有一圈連續的黑色角質脊稜。<br>
• 癩蛤蟆的皮膚與耳後腺：牠們的背部皮膚非常粗糙，佈滿了大小不一的圓形贅疣（疙瘩），每個贅疣的頂端也帶有黑色角質。這種看起來凹凸不平的皮膚，就是民間俗稱牠們為癩蛤蟆的原因。在牠們眼睛後方、鼓膜上方，有一對明顯肥大且鼓起的長橢圓形構造，這在生物學上稱為「耳後腺」。<br>
• 毒性與自我防衛：耳後腺和皮膚的贅疣是黑眶蟾蜍的特殊防衛武器，在受到強烈攻擊或擠壓時，會分泌出白色的毒液（俗稱蟾酥）。<br><br>

<strong>三、生態習性：穩重、慢活的夜間散步者</strong><br>
與善於跳躍、動作敏捷的貢德氏赤蛙不同，黑眶蟾蜍在龍潭大池畔展現出截然不同的生活態度：<br><br>

• 慢條斯理的移動方式：因為身體較為沉重、後肢較短，黑眶蟾蜍非常不擅長跳躍。平時在龍潭大池的木棧道、步道或草叢裡活動時，牠們大多是用「一步一步慢慢爬行」的方式移動。只有在受到突如其來的驚嚇時，才會進行短距離的微小跳躍。<br>
• 繁殖期的急促鳴叫：牠們雖然是陸棲性很強的蟾蜍，平時可以住在遠離水邊的草叢或水溝，但每年春夏兩季（約 3 月到 8 月）的繁殖期，牠們一定會集體遷移到龍潭大池的淺水區。雄蟾蜍擁有單一的咽下外鳴囊，鳴叫時會發出非常急促且連續的「咯咯咯咯咯、囉囉囉囉囉」聲音，聲音清亮且頻率很高。當成百上千隻黑眶蟾蜍在池畔同時鳴叫時，場面相當壯觀。<br><br>

`
},
               'modal-animal-6': {
    t: '草叢喇叭手：小雨蛙',
    b: `🐸 <strong>小雨蛙 (Microhyla fissipes)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
小雨蛙（學名：Microhyla fissipes）屬於姬蛙科姬蛙屬，是台灣原生種的兩棲動物。牠們廣泛分佈於台灣全島低海拔的稻田、埤塘、草澤、溝渠以及人為開發的公園綠地。<br><br>

<strong>二、形態特徵與完美的三角形偽裝</strong><br>
成蛙的體長通常只有 2 到 3 公分：<br><br>

• 獨特的等腰三角形體態：從上方俯瞰小雨蛙，牠們的身體形狀呈現非常標準的「等腰三角形」——頭部尖尖小小的，身體後半段則逐漸變寬。<br>
• 宛如古代山水畫的背部飾紋：這也是牠們被稱為飾紋姬蛙的原因。牠們的背部皮膚非常光滑，底色通常為灰褐色或兩棲類常見的泥土色。<br><br>

<strong>三、生態習性：草叢底層的「隱形超級大喇叭」</strong><br>

• 驚人的鳴叫爆發力：雄性小雨蛙具有一個非常大的單一咽下外鳴囊。當牠們全力鳴叫時，鳴囊會像吹氣球一樣膨脹得比自己的頭還要大。牠們的叫聲是低沉且連續的「低、低、低、低」或類似「嘎、嘎」的粗糙拉長音。<br>
• 喜歡躲藏與雨後狂歡：小雨蛙生性非常羞怯、隱密，平時極少出現在開闊的水面。牠們最喜歡躲在龍潭大池畔密集的草根、泥縫、或是落葉堆的深處。<br><br>

`
},
                'modal-bird-1': {
    t: '水面明星：紅冠水雞',
    b: `🦆 <strong>紅冠水雞 (Gallinula chloropus)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
紅冠水雞（學名：Gallinula chloropus）屬於秧雞科水雞屬，是台灣非常普遍的留鳥（全年定居於台灣）。牠們廣泛分佈於台灣全島低海拔的埤塘、湖泊、河流、濕地以及水稻田。<br><br>

<strong>二、形態特徵與過目不忘的黑身紅額</strong><br>
紅冠水雞的體長大約在 30 到 35 公分之間，：<br><br>

• 招牌的鮮紅額甲與黃色喙尖：這正是牠們被稱為紅冠水雞的原因。牠們的嘴巴（喙）基部與整個額頭，長有一塊鮮紅色的角質外觀，稱為「額甲」。而在嘴巴的最前端，則轉為明亮的鮮黃色。<br>
• 全身羽色與白屁股特徵：牠們的身體主要由黑褐色與灰黑色的羽毛組成。身體兩側（脅部）長有一排非常明顯的白色縱斑，看起來就像是穿了一件帶有白色條紋的西裝。<br>
• 綠色的長腳與蹠骨：雖然牠們花很多時間在水裡游泳，但牠們並不像鴨子那樣長有鴨蹼。牠們的腳趾非常修長，顏色是奇特的黃綠色，在跗關節（膝蓋附近）還有一圈紅色的環帶，非常特別。<br><br>

<strong>三、生態習性：大池水面上的「點頭游泳大師」與友善的家庭觀念</strong><br>

• 獨特的點頭游泳姿態：當紅冠水雞在龍潭大池的水面上前進時，牠們的頭部會隨著划腳的節奏，規律地前後點動，尾巴也會跟著上下翹動。<br>
• 共同育雛的溫馨家族：紅冠水雞是非常稱職的父母，牠們非常喜歡利用大池畔密集的香蒲、風車草等挺水植物築巢。牠們一年可以繁殖多次。<br><br>

`
},
                'modal-bird-2': {
    t: '潛水大師：小鸊鷉',
    b: `🐦 <strong>小鸊鷉 (Tachybaptus ruficollis)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
小鸊鷉（學名：Tachybaptus ruficollis）屬於鸊鷉科小鸊鷉屬，是台灣非常普遍的留鳥（全年定居於台灣）。牠們廣泛分佈於台灣全島低海拔的埤塘、湖泊、水庫及流速緩慢的河流。<br><br>

<strong>二、形態特徵與矮胖如葫蘆的體態</strong><br>
小鸊鷉是台灣體型最小的潛水鳥類，體長約 25 到 28 公分：<br><br>

• 如同水葫蘆般的圓滾滾身軀：小鸊鷉的身體非常矮胖、圓滾，幾乎看不到尾巴（尾羽退化得極短）。<br>
• 隨季節更換的羽色：繁殖期（夏羽）時，臉頰與前頸部會換上耀眼的栗紅色羽毛，嘴基部有一塊明顯黃白色斑塊。非繁殖期（冬羽）則全身轉為樸素灰褐色，是極佳的冬日保護色。<br>
• 特化的瓣蹼足與後置雙腳：不同於鴨子的全蹼，小鸊鷉的腳趾兩側長有像葉片般的扁平皮膜，稱為「瓣蹼」。<br><br>

<strong>三、生態習性：神出鬼沒的「潛水特種部隊」</strong><br>
小鸊鷉最讓遊客驚嘆的，就是牠們近乎出神入化的潛水本領：<br><br>

• 瞬間消失的潛水神功：牠們一憋氣就能在水下待上 20 到 30 秒，再度浮出水面時往往已出現在數十公尺外的角落。<br>
• 奇特的浮巢築巢法：牠們在繁殖期會採集大量枯水草，在香蒲、風車草叢的掩護下，建造漂浮在水面上的「浮巢」。<br><br>

`
},
               'modal-bird-3': {
    t: '水邊紳士：大白鷺',
    b: `🐦 <strong>大白鷺 (Ardea alba)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
大白鷺（學名：Ardea alba）屬於鷺科鷺屬，在台灣主要為普遍的冬候鳥（每年秋季從北方飛來台灣越冬，隔年春季北返），但也有少部分個體成為留鳥。牠們廣泛分佈於全島的濕地、河口、埤塘與水田。<br><br>

<strong>二、形態特徵與極致優雅的黃嘴黑腳</strong><br>
站立時的高度可達 80 到 100 公分，展翅寬度更可達 140 到 170 公分：<br><br>

• 經典的脖子與Ｓ型線條：大白鷺擁有非常修長且靈活的頸部。不論是站立還是飛行時，牠們的脖子經常會縮成非常明顯的Ｓ型。<br>
• 隨季節變色的嘴巴（喙）：大白鷺的嘴巴顏色非常有趣，非繁殖期（冬羽，在台灣最常見的外觀）嘴巴呈現明亮的黃色，但嘴尖有時會帶有一點黑色。到了繁殖期（夏羽，北返前夕），嘴巴會轉變為神祕的黑色，同時眼睛周圍的裸露皮膚會變成美麗的藍綠色（婚姻色），背部也會長出如絲綢般優雅的簑羽。<br>
• 全黑的長腿與腳趾：大白鷺的雙腿非常細長，顏色是乾淨的黑色，連腳趾也全是黑色的。<br><br>

<strong>三、生態習性：大池畔的「靜止型獵捕大師」</strong><br>

• 敵不動、我不動的定身術：大白鷺在捕食時，會緩慢地涉水走到龍潭大池的淺灘、或是風車草與香蒲叢的邊緣。一旦鎖定可能會有魚蝦經過的位置，牠們就會像雕像一樣完全靜止不動，連脖子都會固定住，一站就是好幾分鐘。這種靜止不動的姿態可以降低魚蝦的警覺心。<br>
• 驚人的吞嚥技巧：由於沒有牙齒，大白鷺抓到大魚後，會巧妙地利用嘴巴將魚拋投、甩動，調整角度讓魚頭朝內、魚尾朝外，然後一口氣將整條魚吞下肚子。<br><br>

`
},
                'modal-bird-4': {
    t: '暗光鳥：夜鷺',
    b: `🐦 <strong>夜鷺 (Nycticorax nycticorax)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
夜鷺（學名：Nycticorax nycticorax）屬於鷺科夜鷺屬，在台灣屬於非常普遍的留鳥（全年定居）。牠們廣泛分佈於台灣全島低海拔的埤塘、河流、沼澤、濕地、魚塭以及公園水池。<br><br>

<strong>二、形態特徵：自帶神祕感的「台灣企鵝」</strong><br>
夜鷺的體長大約在 50 到 65 公分之間：<br><br>

• 經典的成鳥羽色：成熟的夜鷺背部與頭頂呈現非常深邃、帶有金屬光澤的藍黑色，雙翅和尾巴是灰色的，而胸部與腹部則是乾淨的白色。br>
• 繁殖期的裝飾——飾羽：在繁殖季節，成年夜鷺的枕骨（後腦勺）會長出 2 到 3 根細長、白色的絲狀羽毛，一直延伸到背部，稱為「飾羽」。。<br>
• 幼鳥的偽裝——星斑羽色：與成鳥完全不同，未成年的夜鷺幼鳥全身都是棕褐色的，且背部密布著許多米黃色的斑點，看起來就像是滿天星星。。<br><br>

<strong>三、生態習性：大池畔的「夜行性捕魚專家」</strong><br>

• 晝伏夜出的暗光鳥：在白天，夜鷺通常顯得懶洋洋的，牠們會成群躲在龍潭大池周邊茂密的樹叢、或是被香蒲與風車草遮蔽的岸邊休息、睡覺。直到黃昏、太陽下山後，牠們才會開始活躍起來。<br>
• 沉穩的伏擊與智慧捕魚：夜鷺捕魚非常有耐心。牠們會蹲低身體，將脖子縮起，靜靜地站在水邊的浮木或石頭上緊盯水面。攻擊時牠們就像一具精準的捕鼠夾，能以極快的速度向前伸長脖子精準夾起游經身邊的魚類。<br><br>

`
},
                'modal-bird-5': {
    t: '藍色閃電：翠鳥',
    b: `🐦 <strong>翠鳥 (Alcedo atthis)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
翠鳥（學名：Alcedo atthis）屬於翠鳥科翠鳥屬，是台灣非常普遍的留鳥。牠們廣泛分佈於台灣全島低海拔的埤塘、湖泊、溪流、溝渠與乾淨的濕地環境。<br><br>

<strong>二、形態特徵：大池畔最亮眼的「藍寶石」</strong><br>
翠鳥的體型非常小巧，體長大約只有 15 到 17 公分：<br><br>

• 令人驚豔的金屬光澤羽色：牠們的背部、翅膀和頭部覆蓋著帶有金屬光澤的翠藍色與輝藍色羽毛，在陽光照射下會折射出像藍寶石般耀眼的光芒。<br>
• 比例誇張的「匕首狀」大嘴：翠鳥擁有一根比例極長、又粗又尖的黑色大嘴巴。<br>
• 口紅辨識法（雌雄小秘密）：公翠鳥的上嘴殼和下嘴殼全都是「黑色」的；而母翠鳥的上嘴殼是黑色的，但下嘴殼的基部會呈現「紅色」或橘紅色。<br><br>

<strong>三、生態習性：時速驚人的「高空魚鷹」與土堤築巢法</strong><br>

• 定點埋伏與高速俯衝：翠鳥習慣孤獨地停在大池畔延伸出水面的枯枝、蘆葦桿或木樁上，眼睛一動不動地凝視著下方的水面。一旦發現小魚游到接近水面的位置，牠們會瞬間收攏雙翅，身體垂直向下。<br>
• 獨特的泥岸水平掘洞築巢：牠們非常依賴大池周邊未被水泥化的自然土堤或沙質岸壁。在繁殖期，翠鳥夫妻會輪流用堅硬的大嘴巴，在垂直的泥岸上橫向挖掘一個直徑約 6 公分、深度可達 50 到 100 公分的水平隧道，並在隧道的盡頭擴大成一個安全的育嬰室。<br><br>

`
},
                'modal-bird-6': {
    t: '瘋狂舞者：小白鷺',
    b: `🐦 <strong>小白鷺 (Egretta garzetta)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
小白鷺（學名：Egretta garzetta）屬於鷺科白鷺屬，是台灣最普遍的留鳥。牠們廣泛分佈於台灣全島低海拔的埤塘、河流、河口、稻田、濕地與魚塭環境。<br><br>

<strong>二、形態特徵與過目不忘的「黃色包鞋」</strong><br>
小白鷺的體長大約在 55 到 65 公分之間：<br><br>

• 經典的黑色大嘴巴：小白鷺的嘴巴（喙）始終呈現乾淨俐落的黑色，質地非常堅硬。<br>
• 最具標誌性的黃色腳趾：小白鷺的雙腿非常細長，顏色是黑色的，但牠們的腳趾卻是極為亮眼的「鮮黃色」或黃綠色。<br>
• 繁殖期的華麗變身：每年春夏季的繁殖期，牠們的枕骨（後腦勺）會長出兩根細長如緞帶的白色飾羽；背部和胸部也會長出像蕾絲般、微微向上捲曲的絲狀蓑羽。<br><br>

<strong>三、生態習性：大池淺灘上的「瘋狂舞蹈家」</strong><br>

• 獨門的「抖腳」驚嚇捕食法：當小白鷺在淺水區或台灣萍蓬草叢旁覓食時，牠們會一邊前進，一邊用那雙鮮黃色的腳掌在水底的泥沙中「瘋狂踩踏、抖動」。<br>
• 拍翅遮光法：白鷺也會在水面上張開雙翅，形成一個小小的遮蔭陰影。這樣做不僅能減少水面的反光、讓牠們更容易看清白天的水下世界。<br><br>

`
},
                'modal-insect-1': {
    t: '鐵甲武士：台灣大鍬形蟲',
    b: `🪲 <strong>台灣大鍬形蟲 (Dorcus grandis formosanus)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
台灣大鍬形蟲（學名：Dorcus grandis formosanus）是巨大鍬形蟲的台灣特有亞種。牠們廣泛分布於台灣本島中、低海拔山區（海拔 200 至 2,000 公尺皆有紀錄）。<br><br>

成蟲主要在每年的 4 月至 9 月活動，其中 6 至 7 月是出沒的高峰期。<br><br>

<strong>二、形態特徵：雄壯的鐵甲武士</strong><br>

• 雄蟲（體長約 24 至 85 mm）：全身散發強烈的黑色光澤。最著名的特徵是其粗壯的大顎，大顎基部（靠近頭部 1/3 處）有一枚明顯發達的齒突，且大顎張開時的弧度非常渾厚、立體。頭部前方兩側各有一個明顯的突起。<br>
• 雌蟲（體長約 33 至 49 mm）：體型比雄蟲更為寬圓，最容易辨識的特徵是牠們的鞘翅（翅膀外殼）上有非常清晰、且排列整齊的縱向點刻條紋。<br><br>

<strong>三、生態與生活習性：夜行與個性對比</strong><br>

• 夜行與趨光性：牠們具有強烈的趨光性，晚上會飛向路燈或光源。不過雄蟲非常有警覺性，飛到光源附近著地後，會迅速尋找陰暗的角落或落葉堆掩蔽。成蟲非常喜歡聚集在殼斗科植物（如青剛櫟、栓皮櫟）的樹幹上，吸食流出的樹液，有時也會久棲在樹洞中。<br>
• 個性上的有趣對比：根據昆蟲愛好者的長期觀察，台灣大鍬形蟲的個性會隨著體型而有奇妙的轉變。中小型雄蟲往往非常的神經質且膽小，遇到驚嚇時逃跑速度極快，而且防衛心重、非常愛夾人；相反地，大型雄蟲個性反而相當穩重、內斂，不常主動發起攻擊。<br><br>
`
},
'modal-insect-2': {
    t: '草叢精靈：纖粉蝶',
    b: `🦋 <strong>纖粉蝶 (Leptosia nina)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
纖粉蝶（學名：Leptosia nina）屬於粉蝶科纖粉蝶屬。牠們主要分布在台灣本島中、低海拔的平原與丘陵地。<br><br>

<strong>二、外觀特徵：樸素卻優雅的微型羽翼</strong><br>
展翅寬大約只有 35 至 40 mm：<br><br>

• 正面純白與招牌黑點：牠們的翅膀正面主要為純白色。最顯眼的特徵是前翅中央有一個明顯的圓形黑斑，且前翅端部（翅尖）有一塊黑色斑紋。<br>
• 背面迷人的大理石紋路：翅膀背面呈現淡淡的白綠色，並佈滿了細密、像大理石紋路般的黃綠色細碎斑紋。<br>
• 纖細圓潤的體態：牠們的翅膀形狀較為圓潤，身體和翅膀結構都顯得相當纖細。<br><br>

<strong>三、生態與生活習性：超好辨識的「靈魂舞步」</strong><br>

• 如夢似幻的飛行方式：纖粉蝶的飛行特徵非常明顯。牠們通常飛得非常低），而且速度極慢、翅膀拍擊節奏輕緩。英文甚至因此稱牠們為 \"Psyche\"（意為靈魂、心靈，以此形容其輕飄飄、如夢似幻的飛行姿態）。<br>
• 專一的食性選擇：成蟲喜歡吸食各種矮小草本植物的微小花蜜。而牠們的幼蟲主要以白花菜科植物為食。<br><br>
`
},
'modal-insect-3': {
    t: '草叢樂手：大草螽',
    b: `🦗 <strong>大草螽 (Conocephalus gigantius)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
大草螽（學名：Conocephalus gigantius）屬於螽斯科草螽屬。牠們廣泛分布於台灣本島的低海拔山區、丘陵以及平地環境。<br><br>

牠們基本上全年可見，但在夏、秋兩季（約 6 月至 10 月）是族群數量最多的時候。<br><br>

<strong>二、外觀特徵：草叢裡的長鬚隱者</strong><br>

• 觸角超長：大草螽的絲狀觸角非常細長，長度往往大幅超過牠們的身體。<br>
• 體色與保護色：牠們的體長大約在 30 至 45 mm 之間，身體主要由鮮綠色與褐色交織而成。牠們的頭部到前胸背板中央，通常有一條明顯的深褐色縱帶。<br>
• 性別分辨的小秘密：雄蟲的翅膀長度通常僅到腹部末端，後翅稍微外露；而雌蟲的腹部末端則長有一根非常明顯、像軍刀一樣的長形產卵管，長度甚至可以達到身體的一半以上。<br><br>

<strong>三、生態與生活習性：夜間機械樂手</strong><br>

• 夜間鳴叫習性：雄蟲會透過左右前翅的摩擦，發出連續且密集的「唧唧唧唧——」或類似機械式的連續鳴聲。<br>
• 雜食偏植食的食性：牠們主要以禾本科植物的嫩葉、種子（如稻穗）為食，但偶爾也會展現捕食性，捕捉其他更弱小的微型昆蟲來補充必要的蛋白質。<br><br>
`
},

'modal-insect-4': {
    t: '潛水獵人：黃綠龍蝨',
    b: `🪲 <strong>黃綠龍蝨 (Cybister tripunctatus lateralis)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
黃綠龍蝨（學名：Cybister tripunctatus lateralis）屬於龍蝨科龍蝨屬。牠們主要棲息在靜水或緩和流動的水田。<br><br>

<strong>二、外觀特徵：流線型的水中潛艇</strong><br>

• 體型與金屬光澤：牠們的體長大約在 24 至 30 mm 之間。背部主要為帶有微弱綠色光澤的黑褐色或深橄欖綠。<br>
• 招牌的黃色邊框：黃綠龍蝨的前胸背板兩側、以及鞘翅的外緣，長有一條非常醒目的黃橙色至黃綠色縱向邊帶。<br>
• 天然的游泳槳：牠們的後腳演化得非常發達且扁平，雙側邊緣長滿了密集的排毛。<br><br>

<strong>三、生態與生活習性：水中的肉食暴君與隨身氧氣筒</strong><br>

• 伏擊獵手與化學武器：黃綠龍蝨成蟲擅長伏擊與追捕水中的小動物，包含水生昆蟲（如蜻蜓稚蟲水蠆）、蝌蚪、小魚，甚至是受傷的蛙類。而牠們的幼蟲（俗稱水蜈蚣）更是恐怖的潛水殺手，擁有一對中空且巨大的鐮刀狀大顎。<br>
• 獨特的鞘翅儲氣呼吸法：龍蝨是用氣管呼吸的昆蟲，不能直接利用水中的溶解氧。會定期游到水面，將腹部末端伸出水面吸氣，並把空气儲存在鞘翅與腹部之間的空隙中。<br>
• 陸海空三棲的夜間遷徙：雖然一生大部分時間都在水中生活，但黃綠龍蝨具有極強的飛行能力。到了夜晚，如果棲息的池塘乾涸，或者為了尋找新的配偶與覓食地，成蟲會在夜間爬出水面並展翅飛向空中。牠們具有強烈的趨光性，晚上常常會飛向平地路燈或路邊店家的光源附近著地。<br><br>
`
},

'modal-insect-5': {
    t: '終極獵手：大刀螳',
    b: `🪲 <strong>大刀螳 (Tenodera aridifolia)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
大刀螳（學名：Tenodera aridifolia）屬於螳螂科大刀螳屬。牠們廣泛分布於台灣平地至低、中海拔山區。成蟲主要在夏、秋兩季最為活躍。<br><br>

憑藉著極具威脅性的巨大捕食足（大刀）與兇猛的獵食性，牠們在草叢與灌木叢生態系中，扮演著居於食物鏈頂層的「頂級掠食者」角色。<br><br>

<strong>二、外觀特徵：草叢中的終極獵手與關鍵辨識點</strong><br>

• 傲人的巨型身材：大刀螳的成蟲體長通常在 70 至 100 mm 之間。<br>
• 多變的擬態體色：綠色型的個體前翅邊緣往往帶有鮮豔的草綠色，能完美融入青翠的草叢；褐色型（枯葉色）則全身呈現如枯草般的縱向斑紋，在秋季的枯枝落葉堆中具有絕佳的擬態保護色。<br>
• 兩足之間的紅色秘密：大刀螳的兩足基部內側通常會帶有明顯的「橙紅色斑」。<br><br>

<strong>三、生態與生活習性：千分之五秒的致命伏擊</strong><br>

• 頂級肉食掠食者：牠們習慣採取「伏擊」戰術，静止不動偽裝成枝葉，當獵物（如蝗蟲、蒼蠅、蝴蝶、蟬）靠近時，會以千分之五秒的驚人速度瞬間彈出大刀，將獵物死死夾住。<br>
• 威嚇防禦機制：當受到嚴重威脅、無法逃跑時，大刀螳會展開雙翅，將前半身直立起來，並透過翅膀互相摩擦發出「沙沙」的警告聲，同時全面張開前足、亮出內側鮮豔的橙紅斑塊，藉此嚇退體型比牠大的掠食者。<br><br>
`
},
'modal-insect-6': {
    t: '紅衣舞者：善變蜻蜓',
    b: `🪲 <strong>善變蜻蜓 (Neurothemis fulvia)</strong><br><br>

<strong>一、基本資料與分類學</strong><br>
善變蜻蜓（學名：Neurothemis fulvia）屬於蜻蜓科純蜻蜓屬。牠們廣泛分布於平地至低海拔的池塘、沼澤、人工生態池、水田、灌溉蓄水池等靜水環境。<br><br>

<strong>二、外觀特徵：熱情的紅色與多變的雌蟲</strong><br>
善變蜻蜓屬於中等體型的蜻蜓，腹長大約 25 至 30 mm，後翅長約 27 至 32 mm：<br><br>

• 成熟雄蟲的紫紅衣裳：成熟的雄蟲全身呈現極為耀眼的深紅色或紫紅色。翅膀上的翅脈非常密集且呈現鮮紅色，只有翅膀最末端的一小部分是透明的。<br>
• 未成熟個體：未成熟的雄蟲與雌蟲身體主要呈現樸素的黃褐色或紅褐色。<br>
• 雌蟲的多樣色型：大致可分為兩種色型：一種是「黃褐色型」，翅膀呈現浪漫的黃褐色，翅端同樣透明；另一種則是「紅色型（又稱擬雄型）」。<br><br>

<strong>三、生態與生活習性：大膽不畏人的草頭隱士</strong><br>

• 不畏人的大膽個性：其中以 4 月至 10 月的夏秋兩季數量最為龐大。牠們很喜歡停棲在水邊的乾枝、蘆葦、甚至是圍籬鐵絲網上。<br>
• 空中攔截食性：牠們屬於純肉食性昆蟲，擁有敏銳的複眼與高超的定懸飛行技巧，主要在空中攔截、捕食蚊子、蠅類、小蛾類等各種微小型飛蟲，是水域周邊優秀的害蟲控制者。<br><br>
`
},
            };

            if(infoData[type]) {
                title.textContent = infoData[type].t;
               body.innerHTML = infoData[type].b;
                modal.style.display = 'flex'; 
                // 💡 關鍵一行：打開彈窗時，鎖死最底層網頁背景的滾動
    document.body.style.overflow = 'hidden'; 
            }
        }

        function closeModal() {
            document.getElementById('customModal').style.display = 'none';
            
    document.body.style.overflow = ''; 
}

       
        // 3. 環境選擇題即時反饋邏輯
        function checkRadio(button, isCorrect, divId, targetText) {
            const box = button.parentElement;
            const allBtns = box.querySelectorAll('.option-btn');
            
            // 清除本題目前的紅綠狀態
            allBtns.forEach(b => b.classList.remove('correct', 'wrong'));

            if (isCorrect) {
                button.classList.add('correct');
            } else {
                button.classList.add('wrong');
                // 自動幫使用者把正確答案標示為綠色
                allBtns.forEach(b => {
                    if(b.textContent.includes(targetText) || b.textContent.startsWith(targetText)) {
                        b.classList.add('correct');
                    }
                });
            }
            // 顯示該題詳盡解析卡片
            document.getElementById(divId).style.display = 'block';
        }

        // 4. 水利填空題即時對錯驗證
        function validateBlanks() {
            const v1 = document.getElementById('bk-1').value.trim();
            const v2 = document.getElementById('bk-2').value.trim();
            const v3 = document.getElementById('bk-3').value.trim();
            const v4 = document.getElementById('bk-4').value.trim();
            const v5 = document.getElementById('bk-5').value.trim();

            let scoreCount = 0;
            if(v1 === '埤塘') scoreCount++;
            if(v2 === '蓄') scoreCount++;
            if(v3 === '放') scoreCount++;
            if(v4 === '水門') scoreCount++;
            if(v5 === '農田') scoreCount++;

            const resultLabel = document.getElementById('bk-result');
            if(scoreCount === 5) {
                resultLabel.style.color = '#10b981';
                resultLabel.textContent = '🎉 全對！完美掌握李芷嫻同學整理的水利工程智慧！';
            } else {
                resultLabel.style.color = '#f97316';
                resultLabel.textContent = `答對了 ${scoreCount} 題。正確完整解答序列為：1.埤塘、2.蓄、3.放、4.水門、5.農田。再試一次吧！`;
            }
        }

        // 5. 申論思考題答案切換
        function toggleRef(id) {
            const block = document.getElementById(id);
            block.style.display = (block.style.display === 'none' || block.style.display === '') ? 'block' : 'none';
        }
        // 修改你點擊打開 Modal 的地方：
        document.querySelectorAll(".dropdown > a").forEach(menu=>{

menu.addEventListener("click",function(e){

    if(window.innerWidth<=768){

        e.preventDefault();

        this.parentNode.classList.toggle("active");

    }

});

});
    </script>
</body>
</html
