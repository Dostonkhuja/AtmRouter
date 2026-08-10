<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
    <title>Qo'qon marshrut Pro (Smart Route)</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.css">
    <style>
        :root{
            --bg:#F2F2F7;--panel:#FFFFFF;--line:rgba(60,60,67,0.12);--line-p:rgba(60,60,67,0.18);
            --ink:#1C1C1E;--ink-dim:#8E8E93;--forest:#16543F;--forest-d:rgba(22,84,63,0.10);
            --amber:#C97A22;--done:#34C759;--done-d:rgba(52,199,89,0.12);--danger:#FF3B30;
            --danger-d:rgba(255,59,48,0.10);--r-lg:18px;--r-md:12px;--r-sm:8px;
            --sh:0 4px 16px rgba(0,0,0,0.04),0 1px 2px rgba(0,0,0,0.02);--sh-active:0 8px 24px rgba(22,84,63,0.18);
        }
        *{box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
        body{margin:0;background:var(--bg);color:var(--ink);font-family:-apple-system,BlinkMacSystemFont,"SF Pro Text","Segoe UI",Roboto,sans-serif;letter-spacing:-0.2px;}
        .mono{font-family:ui-monospace,"SF Mono",monospace;}
        #app{max-width:560px;margin:0 auto;min-height:100vh;display:flex;flex-direction:column;padding-bottom:calc(64px + env(safe-area-inset-bottom));}

        header{position:sticky;top:0;z-index:30;background:rgba(255,255,255,0.88);backdrop-filter:blur(20px);border-bottom:1px solid var(--line);padding:calc(10px + env(safe-area-inset-top)) 16px 12px;display:flex;align-items:center;justify-content:space-between;}
        header h1{margin:0;font-size:17px;font-weight:700;}
        .sub{font-size:12px;color:var(--ink-dim);margin-top:2px;}
        .reset-btn{background:var(--danger-d);border:none;color:var(--danger);font-size:13px;font-weight:600;padding:8px 14px;border-radius:20px;cursor:pointer;transition:all 0.2s;}
        .reset-btn:active{transform:scale(0.95);opacity:0.8;}

        .card{background:var(--panel);margin:12px 12px 8px;border-radius:var(--r-lg);border:1px solid var(--line);padding:16px;box-shadow:var(--sh);}
        .card h2{font-size:12px;text-transform:uppercase;letter-spacing:0.5px;color:var(--ink-dim);margin:0 0 10px;}
        textarea{width:100%;min-height:74px;border:1px solid var(--line);border-radius:var(--r-md);padding:10px 12px;font-size:15px;background:#FAFAFC;color:var(--ink);outline:none;transition:all 0.2s;}
        textarea:focus{border-color:var(--forest);background:#FFF;box-shadow:0 0 0 3px var(--forest-d);}
        .hint{font-size:12px;color:var(--ink-dim);margin-top:6px;}

        .btn{border:none;border-radius:var(--r-md);padding:12px 16px;font-size:15px;font-weight:600;display:inline-flex;align-items:center;justify-content:center;gap:6px;cursor:pointer;transition:all 0.2s cubic-bezier(0.16,1,0.3,1);}
        .btn:active{transform:scale(0.98);}
        .btn-primary{background:var(--forest);color:#fff;width:100%;margin-top:10px;box-shadow:0 4px 12px rgba(22,84,63,0.25);}
        .btn-ghost{background:var(--forest-d);color:var(--forest);}
        .btn-sm{padding:9px 14px;font-size:13px;border-radius:var(--r-sm);}
        .error-box{background:var(--danger-d);color:var(--danger);border-radius:var(--r-md);padding:10px 12px;font-size:13px;margin-top:10px;}

        .quickadd{display:flex;gap:8px;margin:0 12px 10px;background:var(--panel);padding:6px;border-radius:var(--r-lg);border:1px solid var(--line);box-shadow:var(--sh);}
        .quickadd input{flex:1;border:none;padding:8px 12px;font-size:15px;background:transparent;outline:none;}

        .tabs{display:flex;margin:0 12px 10px;background:rgba(118,118,128,0.12);border-radius:12px;padding:3px;position:sticky;top:calc(62px + env(safe-area-inset-top));z-index:25;backdrop-filter:blur(10px);}
        .tab{flex:1;text-align:center;padding:8px 0;font-size:14px;font-weight:600;border-radius:9px;cursor:pointer;transition:all 0.2s;}
        .tab.active{background:#FFF;box-shadow:0 2px 8px rgba(0,0,0,0.12);color:var(--ink);}

        .summary{margin:0 12px 12px;background:var(--panel);border:1px solid var(--line);border-radius:var(--r-lg);padding:12px 16px;display:flex;align-items:center;gap:12px;box-shadow:var(--sh);}
        .summary .stat{flex:1;}
        .summary .stat b{display:block;font-size:17px;}
        .summary .stat span{font-size:11px;color:var(--ink-dim);}

        #listView{padding:0 12px;}
        .stop{display:flex;gap:10px;position:relative;padding-bottom:12px;}
        .stop .track{display:flex;flex-direction:column;align-items:center;width:34px;flex-shrink:0;}
        .seq{width:28px;height:28px;border-radius:50%;background:var(--forest);color:#fff;font-size:12px;font-weight:700;display:flex;align-items:center;justify-content:center;box-shadow:0 2px 6px rgba(0,0,0,0.12);}
        .seq.done{background:var(--done);} .seq.depot{background:var(--amber);} .seq.nocoord{background:var(--danger);}
        .thread{flex:1;width:2px;background:var(--line-p);margin-top:4px;min-height:16px;border-radius:1px;}
        .distchip{font-size:10px;font-weight:700;color:var(--forest);background:var(--forest-d);padding:2px 6px;border-radius:6px;margin:3px 0;white-space:nowrap;}

        .stopcard{flex:1;background:var(--panel);border:1px solid var(--line);border-radius:var(--r-lg);padding:14px;box-shadow:var(--sh);display:flex;gap:12px;position:relative;transition:all 0.25s ease;overflow:hidden;}
        .stopcard.moved-active{transform:scale(0.98) translateY(-2px);border-color:var(--forest);box-shadow:var(--sh-active);background:#F4FDF9;}
        .stopcard.done{background:var(--done-d);border-color:rgba(52,199,89,0.3);}

        .card-content{flex:1;min-width:0;display:flex;flex-direction:column;}
        .stopcard .name{font-size:15px;font-weight:600;padding-right:28px;line-height:1.3;word-break:break-word;}
        .stopcard .ids{font-size:11px;color:var(--ink-dim);margin-top:4px;}

        .remove-btn-top{position:absolute;top:10px;right:10px;background:rgba(118,118,128,0.08);color:var(--ink-dim);border:none;border-radius:50%;width:26px;height:26px;font-size:12px;font-weight:bold;display:flex;align-items:center;justify-content:center;cursor:pointer;z-index:2;transition:all 0.2s;}
        .remove-btn-top:hover{background:var(--danger-d);color:var(--danger);}
        .remove-btn-top:active{transform:scale(0.88);}

        .badge-region,.badge-new,.badge-nocoord{display:inline-block;font-size:10px;font-weight:700;padding:2px 6px;border-radius:6px;margin-left:4px;}
        .badge-region{background:rgba(118,118,128,0.12);color:var(--ink);}
        .badge-new{background:var(--amber);color:#fff;font-size:9px;}
        .badge-nocoord{background:var(--danger);color:#fff;font-size:9px;}

        .actionrow{display:flex;align-items:center;gap:6px;margin-top:12px;flex-wrap:wrap;}
        .iconbtn{background:var(--forest-d);color:var(--forest);border:none;border-radius:var(--r-sm);padding:8px 12px;font-size:13px;font-weight:600;cursor:pointer;min-height:34px;transition:all 0.15s;}
        .iconbtn:active{transform:scale(0.95);}
        .iconbtn.danger{background:var(--danger-d);color:var(--danger);}

        .move-btns{display:flex;flex-direction:column;gap:4px;justify-content:center;align-items:center;background:#FAFAFC;border:1px solid var(--line);padding:4px;border-radius:10px;flex-shrink:0;align-self:center;}
        .move-btn{background:transparent;border:none;border-radius:6px;width:30px;height:32px;font-size:11px;font-weight:bold;color:var(--ink);cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all 0.15s;}
        .move-btn:hover:not(:disabled){background:var(--forest-d);color:var(--forest);}
        .move-btn:active:not(:disabled){background:var(--forest);color:#fff;transform:scale(0.9);}
        .move-btn:disabled{opacity:0.2;cursor:not-allowed;}

        .navlink{font-size:12px;font-weight:600;color:var(--forest);text-decoration:none;background:var(--forest-d);padding:6px 10px;border-radius:var(--r-sm);display:inline-flex;align-items:center;min-height:32px;transition:all 0.15s;}
        .navlink:active{transform:scale(0.95);}
        .navlink.yandex{color:#D90000;background:rgba(217,0,0,0.08);}

        .donetoggle{margin-left:auto;font-size:12px;font-weight:600;padding:6px 12px;border-radius:var(--r-sm);border:1px solid var(--forest);color:var(--forest);background:#fff;cursor:pointer;min-height:32px;transition:all 0.15s;}
        .donetoggle.on{background:var(--done);border-color:var(--done);color:#fff;}
        .donetoggle:active{transform:scale(0.95);}

        .comment-box{margin-top:10px;padding-top:8px;border-top:1px dashed var(--line-p);width:100%;}
        .comment-input{width:100%;border:1px solid var(--line);border-radius:var(--r-sm);padding:7px 10px;font-size:13px;background:#FAFAFC;color:var(--ink);outline:none;transition:all 0.2s;}
        .comment-input:focus{border-color:var(--forest);background:#FFF;box-shadow:0 0 0 2px var(--forest-d);}

        #toast{position:fixed;left:50%;bottom:calc(24px + env(safe-area-inset-bottom));transform:translateX(-50%) translateY(20px);background:rgba(28,28,30,0.92);color:#fff;padding:10px 18px;border-radius:20px;font-size:13px;font-weight:600;opacity:0;pointer-events:none;transition:0.2s;z-index:2000;box-shadow:0 10px 24px rgba(0,0,0,0.2);}
        #toast.show{opacity:1;transform:translateX(-50%) translateY(0);}

        #mapView{padding:0 12px;display:none;}
        #map{width:100%;height:56vh;border-radius:var(--r-lg);border:1px solid var(--line);}
        .mapfoot{margin-top:12px;display:flex;flex-direction:column;gap:8px;}

        .pin{width:28px;height:28px;border-radius:50% 50% 50% 0;transform:rotate(-45deg);background:var(--forest);border:2px solid #fff;display:flex;align-items:center;justify-content:center;box-shadow:0 3px 8px rgba(0,0,0,0.24);}
        .pin span{transform:rotate(45deg);color:#fff;font-size:11px;font-weight:800;}
        .pin.depot{background:var(--amber);} .pin.done{background:var(--done);}

        .history-wrapper{margin:0 12px 12px;}
        .history-toggle-btn{width:100%;background:var(--panel);border:1px solid var(--line);border-radius:var(--r-lg);padding:12px 16px;font-size:14px;font-weight:600;display:flex;align-items:center;justify-content:space-between;cursor:pointer;}
        .history-content{display:none;background:var(--panel);border:1px solid var(--line);border-top:none;border-radius:0 0 var(--r-lg) var(--r-lg);padding:8px 12px 12px;margin-top:-6px;}
        .history-content.open{display:block;}
        .history-item{border-bottom:1px solid var(--line);padding:10px 0;display:flex;align-items:center;justify-content:space-between;}

        .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,0.4);backdrop-filter:blur(4px);display:none;align-items:center;justify-content:center;z-index:1000;padding:20px;}
        .modal-overlay.open{display:flex;}
        .modal-card{background:var(--panel);border-radius:var(--r-lg);width:100%;max-width:360px;padding:20px;border:1px solid var(--line);box-shadow:0 20px 40px rgba(0,0,0,0.15);}
        .modal-card input{width:100%;border:1px solid var(--line);border-radius:var(--r-md);padding:10px 12px;margin-bottom:16px;outline:none;}
        footer{margin:18px 12px 6px;font-size:11px;color:var(--ink-dim);text-align:center;}
    </style>
</head>
<body>
<div id="app">
    <header>
        <div><h1>Qo‘qon bankomat marshruti Pro</h1><div class="sub" id="dateLabel"></div></div>
        <button class="reset-btn" onclick="resetDay()">Yangi kun</button>
    </header>
    <div class="card" id="inputCard">
        <h2>ID kiriting</h2>
        <textarea id="idInput" placeholder="Masalan: 62634 25536 62638..."></textarea>
        <div class="hint">Bo‘sh joy, vergul yoki yangi qator bilan ajrating.</div>
        <button class="btn btn-primary" id="buildBtn" onclick="buildRoute()">Optimal Marshrut tuz</button>
        <div id="errBox"></div>
    </div>
    <div class="quickadd" id="quickAddBar" style="display:none;">
        <input id="quickAddInput" placeholder="Yo‘lda qo‘shish: ID..." onkeydown="if(event.key==='Enter') quickAdd()">
        <button class="btn btn-ghost btn-sm" onclick="quickAdd()">+ Qo‘shish</button>
    </div>
    <div class="tabs" id="tabs" style="display:none;">
        <div class="tab active" id="tabList" onclick="switchTab('list')">Ro‘yxat</div>
        <div class="tab" id="tabMap" onclick="switchTab('map')">Xarita</div>
    </div>
    <div class="summary" id="summary" style="display:none;">
        <div class="stat"><b id="sumDone">0/0</b><span>bajarildi</span></div>
        <div class="stat"><b id="sumDist">0 km</b><span>masofa</span></div>
        <div class="stat"><b id="sumTime">0 daq</b><span>vaqt</span></div>
        <div style="display:flex;gap:4px;">
            <button class="iconbtn" onclick="openSaveModal()">💾</button>
            <button class="iconbtn" onclick="copyRoute()">📋</button>
            <button class="iconbtn danger" onclick="clearRoute()">🗑️</button>
        </div>
    </div>
    <div id="listView"></div>
    <div id="mapView">
        <div id="map"></div>
        <div class="mapfoot">
            <a class="btn btn-primary" id="navAllBtnG" href="#" target="_blank">Google Maps Navigatsiya</a>
            <a class="btn btn-primary" style="background:var(--amber);box-shadow:0 4px 12px rgba(201,122,34,0.25);" id="navAllBtnY" href="#" target="_blank">Yandex Navi Navigatsiya</a>
        </div>
    </div>
    <div class="history-wrapper" id="historyWrapper">
        <button class="history-toggle-btn" onclick="toggleHistoryContent()">
            <span>📂 Saqlangan marshrutlar</span><span id="historyArrow">▼</span>
        </button>
        <div class="history-content" id="historyContent"><div id="historyList"></div></div>
    </div>
    <div class="modal-overlay" id="saveModal">
        <div class="modal-card">
            <h3 style="margin-top:0;">Marshrutni saqlash</h3><p style="font-size:14px;color:var(--ink-dim);">Marshrut nomi:</p>
            <input type="text" id="saveRouteNameInput" placeholder="Masalan: Ertalabki reys">
            <div style="display:flex;gap:8px;justify-content:flex-end;">
                <button class="btn btn-ghost btn-sm" onclick="closeSaveModal()">Bekor qilish</button>
                <button class="btn btn-primary btn-sm" style="margin-top:0;" onclick="confirmSaveRoute()">Saqlash</button>
            </div>
        </div>
    </div>
    <div id="toast"></div>
    <footer>Ma’lumotlar qurilmada saqlanadi. Optimal 2-Opt TSP Algoritmi.</footer>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.js"></script>
<script>
    const $ = id => document.getElementById(id);

    const RAW = [
        [19765,62591,"Kapitalbank filiali Qo‘qon",40.527507,70.939744,"Qo‘qon shahri"],
        [27192,62612,"Юсуфбек",40.508181,70.930420,"Qo‘qon shahri"],
        [25540,62638,"Навоий Марварид",40.541127,70.972689,"Qo‘qon shahri"],
        [20581,62625,"Навоий Беназир",40.539578,70.973586,"Qo‘qon shahri"],
        [25543,62639,"Жиннихона",40.551695,70.961524,"Qo‘qon shahri"],
        [22751,62631,"Эски Беназир",40.543332,70.935198,"Qo‘qon shahri"],
        [25553,62643,"Янги Беназир",40.548711,70.923874,"Qo‘qon shahri"],
        [25528,62632,"Екстренный больница",40.552613,70.930078,"Qo‘qon shahri"],
        [25532,62635,"Мостни таги Янги бозор",40.551276,70.961628,"Qo‘qon shahri"],
        [35116,62645,"Коппон",40.526184,70.955035,"Qo‘qon shahri"],
        [40747,62620,"Al Fajr",40.542249,70.921159,"Qo‘qon shahri"],
        [31212,62614,"Найманча",40.524491,70.874890,"Qo‘qon shahri"],
        [25534,62636,"Қўқонбой",40.469919,71.036609,"Qo‘qon shahri"],
        [25546,62641,"Тўлибой",40.571512,70.949227,"Qo‘qon shahri"],

        [25531,62603,"Furqat bozor",40.485157,70.771771,"Furqat tumani"],
        [20578,62593,"Фурқат 2-мактаб",40.483434,70.768838,"Furqat tumani"],
        [22742,62594,"Фурқат центр Беназир",40.481127,70.771070,"Furqat tumani"],
        [22752,62598,"Томоша",40.469525,70.725567,"Furqat tumani"],
        [22754,62599,"Калдушон",40.472005,70.865062,"Furqat tumani"],
        [25524,62600,"Эшон",40.432663,70.786514,"Furqat tumani"],
        [25526,62602,"Қоратепа",40.496170,70.947387,"Furqat tumani yo'nalishi"],
        [22746,62596,"Катта Янги",40.412961,70.795135,"Furqat tumani"],

        [22749,62597,"Ohunqaynar",40.489739,70.919255,"O‘zbekiston tumani"],
        [25539,62605,"Yaypan Ibn Sino",40.380730,70.807390,"O‘zbekiston tumani"],
        [25551,62608,"Yaypan bozor",40.382123,70.808672,"O‘zbekiston tumani"],
        [35118,62615,"Yaypan Smart",40.380938,70.810247,"O‘zbekiston tumani"],
        [35119,62616,"Yaypan Smart",40.380938,70.810247,"O‘zbekiston tumani"],
        [35121,62617,"Yaypan Smart",40.380938,70.810247,"O‘zbekiston tumani"],
        [25556,62611,"Turon stadion",40.376154,70.825625,"O‘zbekiston tumani"],
        [25555,62610,"Nursuq Xumo market",40.365995,70.749656,"O‘zbekiston tumani"],
        [22745,62595,"Nursux Real Market",40.365995,70.749656,"O‘zbekiston tumani"],
        [20564,62592,"Qudash",40.431528,70.872480,"O‘zbekiston tumani"],
        [31211,62613,"Qushqanoq",40.426288,70.866018,"O‘zbekiston tumani"],
        [25533,62604,"Конизар, туман больница",40.373317,70.834863,"O‘zbekiston tumani"],
        [35122,62618,"Яккатут",40.440822,70.882844,"O‘zbekiston tumani"],
        [25525,62601,"Тагоб",40.363333,70.740183,"O‘zbekiston tumani"],
        [25554,62609,"O‘qchi",40.335926,71.000247,"O‘zbekiston tumani"],

        [25542,62606,"Katta Minglar",40.539945,70.874052,"Dang‘ara tumani"],
        [35125,62647,"Данғара Марварид",40.579433,70.915486,"Dang‘ara tumani"],

        [25536,62637,"Мулкобод",40.669926,70.925951,"Dang‘ara tumani"],
        [25545,62640,"Доимобод",40.644189,70.801574,"Dang‘ara tumani"],
        [25548,62642,"Оқжар",40.667287,70.776388,"Dang‘ara tumani"],
        [20582,62626,"Оқжар Султон",40.672539,70.786122,"Dang‘ara tumani"],

        [25544,62607,"Бекобод",40.491804,70.834974,"Qo‘qon shahri"],

        [35120,62646,"Texnomart Bog‘dod",40.459033,71.213925,"Bog‘dod tumani"],
        [40748,62648,"Палма Боғдод",40.490608,71.108615,"Bog‘dod tumani"],
        [40755,62650,"Бордон",40.521392,71.162372,"Bog‘dod tumani"],

        [40754,62649,"Rishton",40.359448,71.279270,"Rishton tumani"],

        [20574,62621,"Водий Тараққиёт Янги Қўрғон",40.557274,71.142729,"Yangiqo‘rg‘on tumani"],
        [20577,62622,"Yangi Qo‘rg‘on Yulduzcha",40.560428,71.143001,"Yangiqo‘rg‘on tumani"],
        [20583,62627,"Янги Қўрғон бозорча",40.560076,71.142444,"Yangiqo‘rg‘on tumani"],
        [20579,62623,"Чуваланчи",40.492685,71.148291,"Yangiqo‘rg‘on tumani"],

        [35115,62644,"Бувайда круг",40.635463,71.079558,"Buvayda tumani"],
        [25529,62633,"Пошшопирим",40.629550,71.173002,"Buvayda tumani"],
        [22743,62629,"Бештерак",40.616193,71.054612,"Buvayda tumani"],

        [22747,62630,"Асмо Учкўприк",40.544409,71.055668,"Uchko‘prik tumani"],
        [20584,62628,"Sobirjon qishloq",40.618183,70.986767,"Uchko‘prik tumani"],
        [25530,62634,"Қораянтоқ",40.641035,70.998435,"Uchko‘prik tumani"],
        [20580,62624,"Қирғиз",40.416068,71.020548,"Uchko‘prik tumani"],

        [40744,62619,"Рапқон",40.351413,70.666247,"Beshariq tumani"]
    ];

    const DB = RAW.map(r=>({oldId:String(r[0]),newId:String(r[1]),name:r[2],lat:r[3],lng:r[4],hasCoords:r[3]!==null,region:r[5]||"Boshqa"}));
    let currentStartNode = DB.find(a=>a.oldId==="19765"), route = [], map=null, markersLayer=null, polyline=null;

    const norm = s => s.toLowerCase().replace(/[‘’´`]/g,"'").trim();

    const haversine = (a,b) => {
        if(!a.hasCoords || !b.hasCoords) return 0;
        const R=6371, toR=d=>d*Math.PI/180, dL=toR(b.lat-a.lat), dG=toR(b.lng-a.lng);
        const s=Math.sin(dL/2)**2+Math.cos(toR(a.lat))*Math.cos(toR(b.lat))*Math.sin(dG/2)**2;
        return R*2*Math.atan2(Math.sqrt(s),Math.sqrt(1-s));
    };

    function calculateTotalDistance(path) {
        let d = 0;
        for (let i = 0; i < path.length - 1; i++) d += haversine(path[i], path[i+1]);
        return d;
    }

    // ==================================================================================
    // TARIXIY YO'NALISH MODELI — real yurilgan marshrutlardan o'rganilgan bog'liqlik
    // ==================================================================================
    // Har bir massiv — bitta haqiqiy kunlik marshrut, eski ID ketma-ketligi bo'yicha
    // (depodan boshlanib, depoda tugaydi). Yangi kun tugagach shu yerga qo'shib boring —
    // qancha ko'p marshrut to'plansa, algoritm siznikiga shuncha yaqin ishlay boshlaydi.
    const HISTORY_ROUTES = [
        ["19765","22751","25528","25536","25530","35115","25529","40755","40754","20579","25540","25548","19765"],
        ["19765","40747","25546","22743","35115","25529","20583","35120","19765"],
        ["19765","25553","25546","25536","35115","25529","20577","20583","20574","22747","40748","40755","40754","25540","25543","35116","20580","19765"],
        ["19765","22751","35125","25546","25530","22743","20577","20579","40754","20580","25532","19765"],
        ["19765","40747","22751","35116","25540","40748","20579","20574","20583","22743","25543","25546","35125","20584","25536","35115","19765"],
        ["19765","35116","20580","25534","40748","40754","35120","22747","20574","20577","35115","22743","25536","20582","25548","25553","19765"]
    ];

    // A→B nechta marta ketma-ket kelgani (ID juftlik chastotasi)
    const HIST_EDGES = {};
    HISTORY_ROUTES.forEach(seq => {
        for (let i = 0; i < seq.length - 1; i++) {
            const key = seq[i] + '>' + seq[i+1];
            HIST_EDGES[key] = (HIST_EDGES[key] || 0) + 1;
        }
    });

    const HIST_BONUS_PER_MATCH = 6;   // har bir tasdiqlangan bog'liqlik uchun "chegirma" (km) — kerak bo'lsa oshiring/kamaytiring
    const HIST_BONUS_CAP = 20;        // maksimal chegirma (km) — juda uzoq nuqtani majburan yaqinlashtirib yubormasin

    function histBonus(aId, bId){
        const fwd = HIST_EDGES[aId + '>' + bId] || 0;      // shu tartibda ko'rilgan
        const bwd = HIST_EDGES[bId + '>' + aId] || 0;      // teskari tartibda ko'rilgan (zaifroq belgi)
        const weight = fwd + bwd * 0.35;
        return Math.min(HIST_BONUS_CAP, weight * HIST_BONUS_PER_MATCH);
    }

    // Marshrut QURISH uchun ishlatiladigan "narx": haqiqiy masofa - tarixiy bog'liqlik chegirmasi.
    // DIQQAT: haversine() o'zi o'zgarishsiz qoladi — u haqiqiy km/vaqt/xarita/navigatsiya uchun ishlatiladi.
    function routeCost(a, b){
        if(!a.hasCoords || !b.hasCoords) return 0;
        const dist = haversine(a, b);
        return Math.max(0.05, dist - histBonus(a.oldId, b.oldId));
    }
    // ==================================================================================

    function solve2OptTSP(nodes) {
        if (nodes.length <= 2) return nodes;
        let unvisited = nodes.slice(1), current = nodes[0], path = [current];

        while (unvisited.length > 0) {
            let nearestIdx = 0, minCost = Infinity;
            for (let i = 0; i < unvisited.length; i++) {
                let cost = routeCost(current, unvisited[i]);
                if (cost < minCost) { minCost = cost; nearestIdx = i; }
            }
            current = unvisited.splice(nearestIdx, 1)[0];
            path.push(current);
        }

        let improved = true, iter = 0;
        while (improved && iter < 50) {
            improved = false; iter++;
            for (let i = 1; i < path.length - 2; i++) {
                for (let j = i + 1; j < path.length - 1; j++) {
                    let d1 = routeCost(path[i-1], path[i]) + routeCost(path[j], path[j+1]);
                    let d2 = routeCost(path[i-1], path[j]) + routeCost(path[i], path[j+1]);
                    if (d2 < d1 - 0.0001) {
                        path.splice(i, j - i + 1, ...path.slice(i, j + 1).reverse());
                        improved = true;
                    }
                }
            }
        }
        return path;
    }

    const findByToken = tok => {
        const t = tok.trim(); if(!t) return null;
        return /^\d+$/.test(t) ? DB.find(a=>a.oldId===t||a.newId===t) : DB.find(a=>norm(a.name).includes(norm(t)));
    };

    async function buildRoute(){
        const b = $('buildBtn'); b.textContent = "Optimal marshrut tuzilmoqda..."; b.disabled = true;
        const tokens = $('idInput').value.split(/[,\n;]+|\s{1,}/).map(s=>s.trim()).filter(Boolean);
        const found=[], missing=[], seen = new Set();

        tokens.forEach(tok => {
            const item = findByToken(tok);
            if(item && item.oldId !== currentStartNode.oldId && !seen.has(item.oldId)){
                seen.add(item.oldId); found.push({...item, done:false, note:""});
            } else if(!item) missing.push(tok);
        });

        $('errBox').innerHTML = missing.length ? `<div class="error-box">Topilmadi: ${missing.join(", ")}</div>` : "";
        if(!found.length){ route=[]; renderAll(); b.textContent = "Optimal Marshrut tuz"; b.disabled = false; return; }

        const wC = found.filter(a=>a.hasCoords), nC = found.filter(a=>!a.hasCoords);
        let optimizedNodes = solve2OptTSP([currentStartNode, ...wC]);
        optimizedNodes.shift();

        route = [
            {...currentStartNode, done:false, isDepot:true, isStartDepot:true, note:""},
            ...optimizedNodes,
            ...nC.map(a=>({...a,done:false,note:""})),
            {...currentStartNode, oldId: currentStartNode.oldId+"_END", name: currentStartNode.name+" (Qaytish)", done:false, isDepot:true, isReturnDepot:true, note:""}
        ];

        saveState(); renderAll(); toggleControls(true);
        b.textContent = "Optimal Marshrut tuz"; b.disabled = false;
    }

    function quickAdd(){
        const inp = $('quickAddInput'), tokens = inp.value.trim().split(/[,\n;]+|\s{1,}/).map(s=>s.trim()).filter(Boolean);
        let added = 0;
        tokens.forEach(tok => {
            const item = findByToken(tok);
            if(!item || route.some(r=>r.oldId===item.oldId)) return;
            route.splice(route.length > 0 && route[route.length-1].isReturnDepot ? route.length - 1 : route.length, 0, {...item, done:false, justAdded:true, note:""});
            added++;
        });
        if(added) { showToast(`✓ ${added} ta qo‘shildi`); saveState(); renderAll(); inp.value=""; }
    }

    function moveAnimated(idx, dir){
        const t = idx + dir;
        if(t < 1 || t >= route.length || route[t].isDepot) return;
        const cards = document.querySelectorAll('.stopcard');
        if(cards[idx]) cards[idx].classList.add('moved-active');
        if(cards[t]) cards[t].classList.add('moved-active');

        setTimeout(() => {
            [route[idx], route[t]] = [route[t], route[idx]];
            saveState(); renderAll();
        }, 160);
    }

    const toggleDone = idx => { route[idx].done = !route[idx].done; saveState(); renderAll(); };
    const removeStop = idx => { route.splice(idx,1); saveState(); renderAll(); };
    const updateNote = (idx, val) => { route[idx].note = val; saveState(); };

    const toggleHistoryContent = () => { $('historyArrow').textContent = $('historyContent').classList.toggle('open') ? "▲" : "▼"; };
    const openSaveModal = () => { if(!route.length) return alert("Marshrut yo'q!"); $('saveRouteNameInput').value = "Marshrut " + new Date().toLocaleDateString('uz-UZ', {day:'2-digit', month:'2-digit', hour:'2-digit', minute:'2-digit'}); $('saveModal').classList.add('open'); };
    const closeSaveModal = () => $('saveModal').classList.remove('open');

    function confirmSaveRoute(){
        const name = $('saveRouteNameInput').value.trim(); if(!name) return;
        const history = getHistory();
        history.unshift({ id: Date.now().toString(), name, date: new Date().toLocaleDateString('uz-UZ'), count: route.filter(r=>!r.isDepot).length, data: route });
        localStorage.setItem('qoqon_routes_history', JSON.stringify(history));
        showToast("✓ Saqlandi!"); renderHistory(); closeSaveModal();
    }

    const getHistory = () => { try { return JSON.parse(localStorage.getItem('qoqon_routes_history')) || []; } catch { return []; } };

    function loadFromHistory(id){
        const item = getHistory().find(h => h.id === id);
        if(item && confirm("Marshrut almashtirilsinmi?")){ route = item.data; saveState(); renderAll(); toggleControls(true); }
    }

    function deleteFromHistory(id){
        if(confirm("O'chirilsinmi?")){
            localStorage.setItem('qoqon_routes_history', JSON.stringify(getHistory().filter(h => h.id !== id)));
            renderHistory(); showToast("🗑 O'chirildi");
        }
    }

    function renderHistory(){
        const history = getHistory(); $('historyWrapper').style.display = history.length ? 'block' : 'none';
        $('historyList').innerHTML = history.map(i => `
            <div class="history-item">
                <div><b>${i.name}</b><br><small style="color:var(--ink-dim)">${i.date} • ${i.count} ta nuqta</small></div>
                <div><button class="btn btn-ghost btn-sm" onclick="loadFromHistory('${i.id}')">Yuklash</button><button class="iconbtn danger" style="padding:4px 8px;" onclick="deleteFromHistory('${i.id}')">✕</button></div>
            </div>`).join('');
    }

    const fmtKm = km => km < 1 ? Math.round(km*1000)+" m" : km.toFixed(1)+" km";
    const toggleControls = show => ['tabs','summary','quickAddBar'].forEach(id => $(id).style.display = show ? 'flex' : 'none');

    function renderAll(){ renderList(); renderSummary(); renderHistory(); updateNavLinks(); if(map) renderMap(); }

    function renderSummary(){
        if(!route.length) return;
        let dist = calculateTotalDistance(route.filter(r=>r.hasCoords));
        $('sumDone').textContent = `${route.filter(r=>r.done && !r.isDepot).length}/${route.filter(r=>!r.isDepot).length}`;
        $('sumDist').textContent = fmtKm(dist);
        $('sumTime').textContent = Math.round(dist/35*60)+" d";
    }

    function renderList(){
        const el = $('listView');
        if(!route.length) return el.innerHTML = `<div style="text-align:center;color:var(--ink-dim);padding:40px;">Marshrut tuzilmagan.</div>`;
        el.innerHTML = route.map((s, i) => `
            <div class="stop">
                <div class="track">
                    <div class="seq ${s.isDepot?'depot':(!s.hasCoords?'nocoord':(s.done?'done':''))}">${s.isStartDepot?'🏦':(s.isReturnDepot?'🏁':i)}</div>
                    ${(i<route.length-1 && s.hasCoords && route[i+1].hasCoords)?`<div class="distchip">↓ ${fmtKm(haversine(s, route[i+1]))}</div>`:''}
                    ${i<route.length-1?'<div class="thread"></div>':''}
                </div>
                <div class="stopcard ${s.done?'done':''}">
                    ${!s.isDepot ? `<button class="remove-btn-top" onclick="removeStop(${i})" title="O'chirish">✕</button>` : ''}
                    <div class="card-content">
                        <div class="name">${s.name} ${s.region?`<span class="badge-region">${s.region}</span>`:''}${s.justAdded?'<span class="badge-new">YANGI</span>':''}${!s.hasCoords?'<span class="badge-nocoord">NO-COORD</span>':''}</div>
                        <div class="ids mono">eski: ${s.oldId} • yangi: ${s.newId}</div>
                        <div class="comment-box"><input type="text" class="comment-input" placeholder="Izoh kiritish..." value="${s.note||''}" oninput="updateNote(${i}, this.value)"></div>
                        <div class="actionrow">
                            ${s.hasCoords ? `
                                <a class="navlink" href="https://www.google.com/maps/dir/?api=1&destination=${s.lat},${s.lng}" target="_blank">Google Nav</a>
                                <a class="navlink yandex" href="yandexnavi://build_route_on_map?lat_to=${s.lat}&lon_to=${s.lng}" target="_blank">Yandex Nav</a>
                            ` : ''}
                            ${!s.isDepot ? `<button class="donetoggle ${s.done?'on':''}" onclick="toggleDone(${i})">${s.done?'✓ Bajarildi':'Bajarildi?'}</button>` : ''}
                        </div>
                    </div>
                    ${!s.isDepot ? `
                    <div class="move-btns">
                        <button class="move-btn" ${i===1?'disabled':''} onclick="moveAnimated(${i},-1)">▲</button>
                        <button class="move-btn" ${i===route.length-2?'disabled':''} onclick="moveAnimated(${i},1)">▼</button>
                    </div>` : ''}
                </div>
            </div>`).join('');
    }

    function updateNavLinks() {
        const valid = route.filter(r => r.hasCoords);
        if(valid.length < 2) return;

        const origin = `${valid[0].lat},${valid[0].lng}`;
        const destination = `${valid[valid.length-1].lat},${valid[valid.length-1].lng}`;
        const waypoints = valid.slice(1, -1).map(v => `${v.lat},${v.lng}`).join('|');

        $('navAllBtnG').href = `https://www.google.com/maps/dir/?api=1&origin=${origin}&destination=${destination}&waypoints=${waypoints}&travelmode=driving`;
        $('navAllBtnY').href = `yandexnavi://build_route_on_map?lat_from=${valid[0].lat}&lon_from=${valid[0].lng}&lat_to=${valid[valid.length-1].lat}&lon_to=${valid[valid.length-1].lng}`;
    }

    function ensureMap(){
        if(map) return;
        map = L.map('map');
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
        markersLayer = L.layerGroup().addTo(map);
    }

    function renderMap(){
        ensureMap(); markersLayer.clearLayers();
        const pts = route.filter(s=>s.hasCoords), latlngs = pts.map(s=>[s.lat,s.lng]);
        pts.forEach(s => {
            const idx = route.indexOf(s), cls = s.isDepot ? "depot" : (s.done?"done":"");
            const label = s.isStartDepot ? "🏦" : (s.isReturnDepot ? "🏁" : idx);
            L.marker([s.lat,s.lng], {icon: L.divIcon({className:'', html:`<div class="pin ${cls}"><span>${label}</span></div>`, iconSize:[28,28]})}).addTo(markersLayer).bindPopup(`<b>${s.name}</b><br><small>${s.region}</small>`);
        });

        if(polyline) polyline.remove();
        polyline = L.polyline(latlngs, {color:'#16543F', weight:3.5}).addTo(map);
        latlngs.length ? map.fitBounds(polyline.getBounds(), {padding:[30,30]}) : map.setView([40.527507,70.939744], 12);
    }

    function showToast(msg){
        const t = $('toast'); t.textContent = msg; t.classList.add('show');
        setTimeout(()=>t.classList.remove('show'), 1600);
    }

    function clearRoute(){
        if(route.length && confirm("Tozalaninsinmi?")){
            route=[]; localStorage.removeItem("qoqon_route_current");
            $('idInput').value=""; renderAll(); toggleControls(false); showToast("🗑 Tozalandi");
        }
    }

    function switchTab(t){
        $('tabList').classList.toggle('active', t==='list'); $('tabMap').classList.toggle('active', t==='map');
        $('listView').style.display = t==='list' ? 'block':'none'; $('mapView').style.display = t==='map' ? 'block':'none';
        if(t==='map') setTimeout(()=>{ ensureMap(); map.invalidateSize(); renderMap(); }, 30);
    }

    function copyRoute(){
        if(!route.length) return;
        let txt = "Qo‘qon marshruti — " + new Date().toLocaleDateString('uz-UZ') + "\n";
        route.forEach((s,i)=>{ txt += `${s.isDepot?'🏦':i+'.'} ${s.name} (yangi:${s.newId}, eski:${s.oldId}) ${s.note?'['+s.note+']':''}\n`; });
        navigator.clipboard ? navigator.clipboard.writeText(txt).then(()=>showToast("Nusxalandi!")) : prompt("Nusxalash:", txt);
    }

    const saveState = () => { try{ localStorage.setItem("qoqon_route_current", JSON.stringify(route)); }catch{} };
    function loadState(){
        try{
            const raw = localStorage.getItem("qoqon_route_current");
            if(raw){ route = JSON.parse(raw); if(route.length) toggleControls(true); }
        }catch{}
    }
    function resetDay(){ if(confirm("Yangi kun boshlansinmi?")){ localStorage.removeItem("qoqon_route_current"); route=[]; renderAll(); toggleControls(false); } }

    $('dateLabel').textContent = new Date().toLocaleDateString('uz-UZ', {weekday:'long', day:'numeric', month:'long'});
    loadState(); renderAll();
</script>
</body>
</html>
