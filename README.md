[smart-grid-typography-studio-guides-i18n.html](https://github.com/user-attachments/files/26382050/smart-grid-typography-studio-guides-i18n.html)
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Smart Grid Typography Studio · Guides · i18n</title>
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  <script src="https://cdn.jsdelivr.net/npm/fabric@latest/dist/index.min.js"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
  <style>
    html, body { height: 100%; }
    body { font-family: Inter, -apple-system, BlinkMacSystemFont, "SF Pro Text", "PingFang SC", sans-serif; }
    .scrollbar-thin::-webkit-scrollbar { width: 6px; height: 6px; }
    .scrollbar-thin::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.12); border-radius: 999px; }
    .checker {
      background-color: #181818;
      background-image: linear-gradient(45deg, #252525 25%, transparent 25%),
        linear-gradient(-45deg, #252525 25%, transparent 25%),
        linear-gradient(45deg, transparent 75%, #252525 75%),
        linear-gradient(-45deg, transparent 75%, #252525 75%);
      background-size: 14px 14px;
      background-position: 0 0, 0 7px, 7px -7px, -7px 0;
    }
    .layer-row.drag-target { outline: 1px dashed rgba(255,255,255,0.35); }
    @keyframes toastin { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
    .toast-anim { animation: toastin 0.25s ease-out; }
    @keyframes bar { 0% { transform: translateX(-100%); } 100% { transform: translateX(200%); } }
    .progress-shim { animation: bar 1.1s ease-in-out infinite; }
    .ruler-top {
      height: 22px; min-width: 40px;
      background: #2d2d2d; border-bottom: 1px solid rgba(255,255,255,0.12);
      position: relative; box-sizing: border-box;
      background-image: repeating-linear-gradient(90deg, transparent 0, transparent 9px, rgba(255,255,255,0.06) 9px, rgba(255,255,255,0.06) 10px);
    }
    .ruler-left {
      width: 22px; min-height: 40px;
      background: #2d2d2d; border-right: 1px solid rgba(255,255,255,0.12);
      position: relative; box-sizing: border-box;
      background-image: repeating-linear-gradient(0deg, transparent 0, transparent 9px, rgba(255,255,255,0.06) 9px, rgba(255,255,255,0.06) 10px);
    }
    .ruler-corner {
      width: 22px; height: 22px; flex-shrink: 0;
      background: #252525; border-right: 1px solid rgba(255,255,255,0.12); border-bottom: 1px solid rgba(255,255,255,0.12);
    }
    .ruler-tick-h { position: absolute; bottom: 0; width: 0; border-left: 1px solid rgba(255,255,255,0.22); font-size: 8px; color: #A0A0A0; padding-left: 2px; line-height: 1; }
    .ruler-tick-v { position: absolute; right: 0; height: 0; border-top: 1px solid rgba(255,255,255,0.22); font-size: 8px; color: #A0A0A0; padding-right: 2px; text-align: right; line-height: 1; }
    #canvasShell.rulers-hidden #rulerTopRow,
    #canvasShell.rulers-hidden #rulerLeftWrap { display: none !important; }
  </style>
</head>
<body class="bg-[#1E1E1E] text-white antialiased overflow-hidden">

  <div id="toast" class="fixed bottom-6 left-1/2 -translate-x-1/2 z-[100] pointer-events-none hidden px-4 py-2 rounded-xl bg-white/15 border border-white/15 backdrop-blur-md text-sm text-white/95 shadow-lg toast-anim"></div>

  <div id="exportModal" class="fixed inset-0 z-[90] hidden items-center justify-center bg-black/50 backdrop-blur-sm p-4">
    <div class="w-full max-w-md rounded-2xl bg-[#2D2D2D]/95 border border-white/10 shadow-[0_24px_80px_rgba(0,0,0,0.55)] backdrop-blur-xl p-6">
      <div class="flex justify-between items-start mb-4">
        <h2 class="text-lg font-semibold tracking-tight">Export</h2>
        <button type="button" id="exportClose" class="text-[#A0A0A0] hover:text-white text-2xl leading-none w-8 h-8 rounded-lg hover:bg-white/10">&times;</button>
      </div>
      <p class="text-xs text-[#A0A0A0] mb-4">PNG / JPG 為真實點陣。.ai / .psd 為 JSON 示範檔。</p>
      <div class="grid grid-cols-2 gap-2 mb-4">
        <button type="button" data-fmt="png" class="ex-btn rounded-xl border border-white/10 bg-white/5 hover:bg-white/10 p-3 text-left text-sm transition">PNG<span class="block text-[10px] text-[#A0A0A0] mt-1">透明</span></button>
        <button type="button" data-fmt="jpg" class="ex-btn rounded-xl border border-white/10 bg-white/5 hover:bg-white/10 p-3 text-left text-sm transition">JPG</button>
        <button type="button" data-fmt="ai" class="ex-btn rounded-xl border border-white/10 bg-white/5 hover:bg-white/10 p-3 text-left text-sm transition">.ai.json</button>
        <button type="button" data-fmt="psd" class="ex-btn rounded-xl border border-white/10 bg-white/5 hover:bg-white/10 p-3 text-left text-sm transition">.psd.json</button>
      </div>
      <div id="exportProgress" class="hidden h-1.5 rounded-full bg-white/10 overflow-hidden mb-2">
        <div class="h-full w-1/3 rounded-full bg-white/80 progress-shim"></div>
      </div>
      <div id="exportMsg" class="text-xs text-[#A0A0A0] min-h-4"></div>
    </div>
  </div>

  <header class="h-11 flex items-center px-3 border-b border-white/10 bg-[#2D2D2D]/80 backdrop-blur-xl shrink-0 z-30">
    <div class="flex items-center gap-1.5 shrink-0">
      <span class="w-3 h-3 rounded-full bg-[#FF5F57]"></span>
      <span class="w-3 h-3 rounded-full bg-[#FEBC2E]"></span>
      <span class="w-3 h-3 rounded-full bg-[#28C840]"></span>
    </div>
    <h1 id="appTitle" class="ml-4 flex-1 text-center text-sm font-medium text-white/90 truncate pointer-events-none">Smart Grid Typography Studio</h1>
    <div class="shrink-0 flex items-center gap-1 flex-wrap justify-end max-w-[min(520px,46vw)]">
      <label class="flex items-center gap-1 text-[10px] text-[#A0A0A0] whitespace-nowrap cursor-pointer select-none" title="像素標尺">
        <input type="checkbox" id="chkRulers" class="rounded border-white/25 accent-[#9BD7BB]" checked />
        標尺
      </label>
      <label class="flex items-center gap-1 text-[10px] text-[#A0A0A0] whitespace-nowrap cursor-pointer select-none" title="依目前網格系統顯示對齊線（在所有圖層之上）">
        <input type="checkbox" id="chkAlignGrid" class="rounded border-white/25 accent-[#9BD7BB]" checked />
        對齊線
      </label>
      <select id="selGuidePalette" class="rounded-lg bg-[#1E1E1E] border border-white/10 text-[10px] py-1 px-1.5 text-[#C8C8C8] max-w-[7.5rem]" title="參考線色系">
        <option value="mint">薄荷綠（類 AI）</option>
        <option value="gray">淺灰</option>
      </select>
      <button type="button" id="btnLang"
        class="px-2.5 py-1.5 text-xs font-medium rounded-lg bg-white/5 hover:bg-white/10 border border-white/10 transition text-[#C8C8C8]"
        title="切換語言 / Switch language">中文</button>
      <button type="button" id="btnUndo" disabled
        class="px-2.5 py-1.5 text-xs font-medium rounded-lg bg-white/10 hover:bg-white/15 border border-white/10 transition opacity-40 cursor-not-allowed disabled:hover:bg-white/10"
        title="撤回上一步（⌘Z / Ctrl+Z）">撤回</button>
      <button type="button" id="btnExport" class="px-3 py-1.5 text-xs font-medium rounded-lg bg-white/10 hover:bg-white/15 border border-white/10 transition">Export</button>
    </div>
  </header>

  <div class="flex h-[calc(100vh-2.75rem)]">
    <aside class="w-64 border-r border-white/10 bg-[#2D2D2D]/60 backdrop-blur-xl flex flex-col shrink-0">
      <div id="hdrLayers" class="px-3 py-2 border-b border-white/10 text-[10px] uppercase tracking-widest text-[#A0A0A0] font-medium">Layers</div>
      <div id="layerList" class="flex-1 overflow-y-auto scrollbar-thin p-2 space-y-1 min-h-0"></div>
      <div class="p-2 border-t border-white/10 space-y-2">
        <button type="button" id="btnAddImage" class="w-full py-2 text-xs font-medium rounded-lg bg-white/10 hover:bg-white/15 border border-white/10 transition">Add Image</button>
        <button type="button" id="btnAddText" class="w-full py-2 text-xs font-medium rounded-lg bg-white/10 hover:bg-white/15 border border-white/10 transition">Add Text Layer</button>
        <button type="button" id="btnMerge" class="w-full py-2 text-xs font-medium rounded-lg bg-white/5 hover:bg-white/10 border border-white/10 text-[#A0A0A0] hover:text-white transition">Merge Selected Layers</button>
      </div>
      <input type="file" id="fileInput" accept="image/*" multiple class="hidden" />
    </aside>

    <main class="flex-1 flex flex-col min-w-0 min-h-0 bg-[#1E1E1E]">
      <div id="viewport" class="checker flex-1 flex items-start justify-center overflow-x-auto overflow-y-auto min-h-0 relative cursor-grab active:cursor-grabbing p-3 scrollbar-thin">
        <div id="canvasShell" class="rounded-xl overflow-visible shadow-[0_20px_60px_rgba(0,0,0,0.55)] ring-1 ring-white/10 flex flex-col bg-[#252525]">
          <div id="rulerTopRow" class="flex">
            <div class="ruler-corner" title="px"></div>
            <div id="rulerTop" class="ruler-top"></div>
          </div>
          <div class="flex">
            <div id="rulerLeftWrap"><div id="rulerLeft" class="ruler-left"></div></div>
            <div class="rounded-br-xl overflow-hidden">
              <canvas id="artboard" width="900" height="600"></canvas>
            </div>
          </div>
        </div>
      </div>
      <p id="footerHint" class="text-[10px] text-[#A0A0A0] text-center py-1 border-t border-white/5 px-2">
        排版／網格以<strong class="text-white/70">選取的圖片</strong>為範圍；淡色背景格＋置頂對齊線隨預設變化 · 標尺／對齊線可關閉 · 滾輪縮放 / 中鍵平移
      </p>
    </main>

    <aside class="w-80 border-l border-white/10 bg-[#2D2D2D]/60 backdrop-blur-xl flex flex-col shrink-0">
      <div id="hdrPresets" class="px-3 py-2 border-b border-white/10 text-[10px] uppercase tracking-widest text-[#A0A0A0] font-medium">Typography presets</div>
      <div id="presetHost" class="flex-1 overflow-y-auto scrollbar-thin p-3 space-y-2 min-h-0"></div>
      <div class="border-t border-white/10">
        <div id="hdrProps" class="px-3 py-2 text-[10px] uppercase tracking-widest text-[#A0A0A0]">Properties</div>
        <div id="propPanel" class="p-3 space-y-3 pb-5 max-h-[42vh] overflow-y-auto scrollbar-thin opacity-40 pointer-events-none">
          <div>
            <label class="text-[10px] text-[#A0A0A0]">Font</label>
            <select id="pfFont" class="mt-1 w-full rounded-lg bg-[#1E1E1E] border border-white/10 text-xs py-1.5 px-2"></select>
          </div>
          <div>
            <label class="text-[10px] text-[#A0A0A0]">Size</label>
            <div class="flex gap-2 mt-1 items-center">
              <input type="range" id="pfSizeR" min="8" max="160" class="flex-1 accent-white" />
              <input type="number" id="pfSizeN" min="8" max="200" class="w-14 rounded-lg bg-[#1E1E1E] border border-white/10 text-xs py-1 px-1.5" />
            </div>
          </div>
          <div class="grid grid-cols-3 gap-2">
            <div><label class="text-[10px] text-[#A0A0A0]">R</label><input type="number" id="pfR" min="0" max="255" class="w-full mt-1 rounded-lg bg-[#1E1E1E] border border-white/10 text-xs py-1 px-1.5" /></div>
            <div><label class="text-[10px] text-[#A0A0A0]">G</label><input type="number" id="pfG" min="0" max="255" class="w-full mt-1 rounded-lg bg-[#1E1E1E] border border-white/10 text-xs py-1 px-1.5" /></div>
            <div><label class="text-[10px] text-[#A0A0A0]">B</label><input type="number" id="pfB" min="0" max="255" class="w-full mt-1 rounded-lg bg-[#1E1E1E] border border-white/10 text-xs py-1 px-1.5" /></div>
          </div>
          <input type="color" id="pfHex" class="w-full h-8 rounded-lg border border-white/10 bg-transparent cursor-pointer" />
          <div>
            <label class="text-[10px] text-[#A0A0A0]">Opacity</label>
            <input type="range" id="pfOp" min="0" max="100" class="w-full mt-1 accent-white" />
          </div>
          <div>
            <label class="text-[10px] text-[#A0A0A0]">Line height</label>
            <input type="range" id="pfLH" min="80" max="220" class="w-full mt-1 accent-white" />
          </div>
          <div>
            <label class="text-[10px] text-[#A0A0A0]">Letter spacing</label>
            <input type="range" id="pfCh" min="-50" max="200" class="w-full mt-1 accent-white" />
          </div>
          <div id="typoLockRow" class="hidden pt-1 border-t border-white/10">
            <label class="flex items-center gap-2 text-[10px] text-[#A0A0A0] cursor-pointer">
              <input type="checkbox" id="pfUnlockPos" class="rounded border-white/20" />
              允許拖移文字位置（版型建議預設鎖定）
            </label>
          </div>
        </div>
      </div>
    </aside>
  </div>

  <script>
    document.addEventListener("DOMContentLoaded", function () {
      try {
        if (typeof tailwind !== "undefined") {
          tailwind.config = { content: [], theme: { extend: {} } };
        }
      } catch (e) {}

      var DEFAULT_W = 900;
      var DEFAULT_H = 600;
      var MAX_ART = 1400;
      var MIN_ART = 280;

      var artW = DEFAULT_W;
      var artH = DEFAULT_H;
      var currentPresetId = "thirds";
      var currentLang = (function () {
        try {
          var l = (navigator.language || "").toLowerCase();
          return l.startsWith("zh") ? "zh" : "en";
        } catch (e) { return "zh"; }
      })();

      var I18N = {
        zh: {
          appTitle: "Smart Grid Typography Studio",
          btnUndo: "撤回",
          btnExport: "匯出",
          exportTitle: "匯出",
          exportDesc: "PNG / JPG 為真實點陣。.ai / .psd 為 JSON 示範檔。",
          exportTransparent: "透明",
          hdrLayers: "圖層",
          btnAddImage: "新增圖片",
          btnAddText: "新增文字圖層",
          btnMerge: "合併選取圖層",
          hdrPresets: "排版預設",
          hdrProps: "屬性",
          footerHint: "排版／網格以<strong class=\"text-white/70\">選取的圖片</strong>為範圍；淡色背景格＋置頂對齊線隨預設變化 · 標尺／對齊線可關閉 · 滾輪縮放 / 中鍵平移",
          chkRulers: "標尺",
          chkRulersTitle: "像素標尺",
          chkAlignGrid: "對齊線",
          chkAlignGridTitle: "依目前網格系統顯示對齊線（在所有圖層之上）",
          paletteTitle: "參考線色系",
          paletteMint: "薄荷綠（類 AI）",
          paletteGray: "淺灰",
          btnLang: "中文",
          btnLangTitle: "切換語言 / Switch language",
          presetScopeWithRef: function (name) { return "版型範圍：<span class=\"text-white/80\">" + name + "</span> 外框內"; },
          presetScopeNoRef: "尚無圖片 · 預覽以整張畫布計算 · 匯入後請選取圖片再套用",
          presetApplyHint: "點擊套用（鎖定位）",
          presetMetaFont: "字體",
          presetMetaSize: "大小",
          presetMetaRgb: "RGB",
          toastNeedImage: "請先匯入圖片，並在畫布上點選要排版的圖層",
          alertNeedMulti: "請框選兩個以上圖層（Shift + 點選多個）。",
          alertNeedTwo: "請至少選取兩個物件。",
          alertMergeFail: "合併失敗。",
          toastExportOk: "匯出成功",
          toastExportFail: "匯出失敗",
          exportWorking: "處理中…",
          exportDownloaded: "已下載。",
          propFont: "字體",
          propSize: "大小",
          propOpacity: "透明度",
          propLineHeight: "行高",
          propLetterSpacing: "字距",
          propAllowMove: "允許拖移文字位置（版型建議預設鎖定）"
        },
        en: {
          appTitle: "Smart Grid Typography Studio",
          btnUndo: "Undo",
          btnExport: "Export",
          exportTitle: "Export",
          exportDesc: "PNG/JPG export raster images. .ai/.psd are JSON mock files.",
          exportTransparent: "Transparent",
          hdrLayers: "Layers",
          btnAddImage: "Add Image",
          btnAddText: "Add Text Layer",
          btnMerge: "Merge Selected Layers",
          hdrPresets: "Typography presets",
          hdrProps: "Properties",
          footerHint: "Typography/grid scope uses the <strong class=\"text-white/70\">selected image</strong>; subtle background grid + top alignment guides follow the preset · Toggle rulers/guides · Wheel to zoom / Middle-button to pan",
          chkRulers: "Rulers",
          chkRulersTitle: "Pixel rulers",
          chkAlignGrid: "Guides",
          chkAlignGridTitle: "Show alignment guides (always on top)",
          paletteTitle: "Guide palette",
          paletteMint: "Mint (AI-like)",
          paletteGray: "Light gray",
          btnLang: "EN",
          btnLangTitle: "Switch language / 切換語言",
          presetScopeWithRef: function (name) { return "Scope: inside <span class=\"text-white/80\">" + name + "</span> bounds"; },
          presetScopeNoRef: "No images yet · Preview uses the whole artboard · Import an image and select it to apply",
          presetApplyHint: "Click to apply (locks position)",
          presetMetaFont: "Font",
          presetMetaSize: "Size",
          presetMetaRgb: "RGB",
          toastNeedImage: "Import an image first, then click the target layer on canvas.",
          alertNeedMulti: "Please select at least two layers (Shift + click multiple).",
          alertNeedTwo: "Please select at least two objects.",
          alertMergeFail: "Merge failed.",
          toastExportOk: "Export successful",
          toastExportFail: "Export failed",
          exportWorking: "Working…",
          exportDownloaded: "Downloaded.",
          propFont: "Font",
          propSize: "Size",
          propOpacity: "Opacity",
          propLineHeight: "Line height",
          propLetterSpacing: "Letter spacing",
          propAllowMove: "Allow moving text (presets lock by default)"
        }
      };

      function t(key) {
        var pack = I18N[currentLang] || I18N.zh;
        var v = pack[key];
        return v == null ? key : v;
      }

      function presetTitle(pr) {
        var pack = I18N[currentLang] || I18N.zh;
        if (currentLang === "en") return pr.titleEn || pr.titleZh || pr.id;
        return pr.titleZh || pr.titleEn || pr.id;
      }

      function presetSubtitle(pr) {
        if (currentLang === "en") return pr.subtitleEn || pr.subtitleZh || "";
        return pr.subtitleZh || pr.subtitleEn || "";
      }

      function applyLanguage() {
        try { document.documentElement.lang = currentLang === "en" ? "en" : "zh-Hant"; } catch (e) {}
        var pack = I18N[currentLang] || I18N.zh;
        var el;

        el = document.getElementById("appTitle");
        if (el) el.textContent = pack.appTitle;
        el = document.getElementById("hdrLayers");
        if (el) el.textContent = pack.hdrLayers;
        el = document.getElementById("hdrPresets");
        if (el) el.textContent = pack.hdrPresets;
        el = document.getElementById("hdrProps");
        if (el) el.textContent = pack.hdrProps;
        el = document.getElementById("footerHint");
        if (el) el.innerHTML = pack.footerHint;

        el = document.getElementById("btnUndo");
        if (el) el.textContent = pack.btnUndo;
        el = document.getElementById("btnExport");
        if (el) el.textContent = pack.btnExport;

        el = document.getElementById("btnAddImage");
        if (el) el.textContent = pack.btnAddImage;
        el = document.getElementById("btnAddText");
        if (el) el.textContent = pack.btnAddText;
        el = document.getElementById("btnMerge");
        if (el) el.textContent = pack.btnMerge;

        el = document.getElementById("btnLang");
        if (el) { el.textContent = pack.btnLang; el.title = pack.btnLangTitle; }

        el = document.getElementById("chkRulers");
        if (el && el.parentElement) el.parentElement.title = pack.chkRulersTitle;
        if (el && el.parentElement) {
          var nodes = el.parentElement.childNodes;
          for (var i = 0; i < nodes.length; i++) {
            if (nodes[i].nodeType === 3) nodes[i].textContent = " " + pack.chkRulers;
          }
        }

        el = document.getElementById("chkAlignGrid");
        if (el && el.parentElement) el.parentElement.title = pack.chkAlignGridTitle;
        if (el && el.parentElement) {
          var nodes2 = el.parentElement.childNodes;
          for (var j = 0; j < nodes2.length; j++) {
            if (nodes2[j].nodeType === 3) nodes2[j].textContent = " " + pack.chkAlignGrid;
          }
        }

        el = document.getElementById("selGuidePalette");
        if (el) el.title = pack.paletteTitle;
        var optMint = el && el.querySelector('option[value="mint"]');
        var optGray = el && el.querySelector('option[value="gray"]');
        if (optMint) optMint.textContent = pack.paletteMint;
        if (optGray) optGray.textContent = pack.paletteGray;

        var expTitle = document.querySelector("#exportModal h2");
        if (expTitle) expTitle.textContent = pack.exportTitle;
        var expDesc = document.querySelector("#exportModal p");
        if (expDesc) expDesc.textContent = pack.exportDesc;
        var expPng = document.querySelector('#exportModal button[data-fmt="png"] span');
        if (expPng) expPng.textContent = pack.exportTransparent;

        var labFont = document.querySelector('label[for="pfFont"], #propPanel label');
        var labels = document.querySelectorAll("#propPanel label.text-\\[10px\\]");
        if (labels && labels.length) {
          // Keep minimal: update the first few known labels by order.
          if (labels[0]) labels[0].textContent = pack.propFont;
          if (labels[1]) labels[1].textContent = pack.propSize;
          if (labels[5]) labels[5].textContent = pack.propOpacity;
          if (labels[6]) labels[6].textContent = pack.propLineHeight;
          if (labels[7]) labels[7].textContent = pack.propLetterSpacing;
        }
        var allowMove = document.querySelector('#typoLockRow label');
        if (allowMove) {
          var txtNodes = allowMove.childNodes;
          for (var k = 0; k < txtNodes.length; k++) {
            if (txtNodes[k].nodeType === 3) txtNodes[k].textContent = "\n              " + pack.propAllowMove + "\n            ";
          }
        }

        buildPresets();
      }

      var FONTS = [
        "Inter", "PingFang SC", "PingFang TC", "SF Pro Display", "SF Pro Text",
        "Helvetica Neue", "Helvetica", "Arial", "Roboto", "Noto Sans SC",
        "Noto Sans TC", "Microsoft YaHei", "system-ui", "sans-serif"
      ];

      var PRESETS = [
        { id: "thirds", titleZh: "三分法", subtitleZh: "交點與能量線", titleEn: "Rule of Thirds", subtitleEn: "Intersections & energy lines" },
        { id: "golden", titleZh: "黃金比例", subtitleZh: "螺旋動線", titleEn: "Golden Ratio", subtitleEn: "Spiral flow" },
        { id: "center", titleZh: "中軸平衡", subtitleZh: "中軸對稱", titleEn: "Centered Balance", subtitleEn: "Axis symmetry" },
        { id: "asym", titleZh: "非對稱平衡", subtitleZh: "張力與重心", titleEn: "Asymmetrical Balance", subtitleEn: "Tension & visual weight" },
        { id: "modular", titleZh: "12 欄模組", subtitleZh: "欄位系統", titleEn: "12-Column Modular", subtitleEn: "Column system" },
        { id: "edge", titleZh: "邊界極簡", subtitleZh: "靠邊對齊", titleEn: "Edge-Aligned Minimal", subtitleEn: "Minimal edge alignment" },
        { id: "axis", titleZh: "軸線式", subtitleZh: "主軸 · 輔軸 · 基線", titleEn: "Axis-Based", subtitleEn: "Primary axis, secondary axes, baselines" },
        { id: "radial", titleZh: "放射式", subtitleZh: "中心往邊界輻射", titleEn: "Radial", subtitleEn: "Radiating from center" },
        { id: "random_grid", titleZh: "隨機式", subtitleZh: "可重現的假隨機分割", titleEn: "Randomized", subtitleEn: "Repeatable pseudo-random splits" },
        { id: "marching", titleZh: "行進式", subtitleZh: "分帶橫線 · 錯位直線", titleEn: "Marching", subtitleEn: "Banded lanes with staggered rhythm" },
        { id: "mod_square", titleZh: "模組式", subtitleZh: "等距單元格（欄×列）", titleEn: "Module Grid", subtitleEn: "Even modules (cols × rows)" },
        { id: "bilateral", titleZh: "雙邊對稱式", subtitleZh: "中央鏡像 · 成對輔線", titleEn: "Bilateral Symmetry", subtitleEn: "Mirrored pairs around center" }
      ];

      if (typeof fabric === "undefined") {
        var msg = currentLang === "en"
          ? "Fabric.js failed to load. Please check your network."
          : "Fabric.js 未載入，請檢查網路。";
        document.body.innerHTML = '<div class="p-8 text-center text-red-300">' + msg + "</div>";
        return;
      }

      var F = fabric;
      var Canvas = F.Canvas;
      var FabricImage = F.FabricImage || F.Image;
      var Textbox = F.Textbox || F.IText;
      var Group = F.Group;
      var Line = F.Line;
      var Control = F.Control;

      if (!Canvas || !FabricImage || !Textbox || !Group || !Line) {
        document.body.innerHTML += '<div class="fixed inset-0 flex items-center justify-center bg-black/90 text-red-300 z-[200] p-4 text-sm">Fabric 模組不完整</div>';
        return;
      }

      console.log("Fabric initialized");

      var canvas = null;
      var gridGroup = null;
      var backGridGroup = null;
      var rulersVisible = true;
      var alignGridVisible = true;
      var guidePaletteKey = "mint";
      var GUIDE_PALETTES = {
        mint: { back: "rgba(130, 200, 175, 0.09)", front: "rgba(155, 215, 188, 0.38)" },
        gray: { back: "rgba(255, 255, 255, 0.045)", front: "rgba(210, 212, 218, 0.34)" }
      };
      var imageSerial = 0;
      var analysis = { lum: 0.5, cx: 0.5, cy: 0.5, ar: 200, ag: 200, ab: 200 };
      var spaceDown = false;
      var isPan = false;
      var lastPt = { x: 0, y: 0 };

      var HISTORY_MAX = 45;
      var HISTORY_JSON_PROPS = ["layerId", "layerType", "layerLabel", "typographyLocked", "lockMovementX", "lockMovementY"];
      var historyStack = [];
      var historyIndex = -1;
      var historyPaused = false;
      var silentHistory = 0;
      var historyPushTimer = null;

      var probe = document.createElement("canvas");
      probe.width = 48;
      probe.height = 48;
      var pctx = probe.getContext("2d", { willReadFrequently: true });

      function genId() {
        return "L_" + Math.random().toString(36).slice(2, 8) + Date.now().toString(36).slice(-4);
      }

      function showToast(msg) {
        var t = document.getElementById("toast");
        t.textContent = msg;
        t.classList.remove("hidden");
        clearTimeout(showToast._tm);
        showToast._tm = setTimeout(function () { t.classList.add("hidden"); }, 2500);
      }

      function safeRender() {
        try {
          if (canvas.renderAll) canvas.renderAll();
          else if (canvas.requestRenderAll) canvas.requestRenderAll();
        } catch (e) { console.warn(e); }
      }

      function updateUndoButton() {
        var btn = document.getElementById("btnUndo");
        if (!btn) return;
        var can = !!(canvas && historyIndex > 0);
        btn.disabled = !can;
        btn.classList.toggle("opacity-40", !can);
        btn.classList.toggle("cursor-not-allowed", !can);
      }

      function serializeCanvasState() {
        try {
          return JSON.stringify(canvas.toJSON(HISTORY_JSON_PROPS));
        } catch (e) {
          console.warn("serializeCanvasState", e);
          return null;
        }
      }

      function pushHistory() {
        if (historyPaused || !canvas || silentHistory > 0) return;
        var json = serializeCanvasState();
        if (!json) return;
        if (historyIndex >= 0 && historyStack[historyIndex] === json) return;
        historyStack = historyStack.slice(0, historyIndex + 1);
        historyStack.push(json);
        historyIndex = historyStack.length - 1;
        while (historyStack.length > HISTORY_MAX) {
          historyStack.shift();
          historyIndex--;
        }
        updateUndoButton();
      }

      function scheduleHistoryPush() {
        if (historyPaused || silentHistory > 0 || !canvas) return;
        clearTimeout(historyPushTimer);
        historyPushTimer = setTimeout(function () {
          if (historyPaused || silentHistory > 0 || !canvas) return;
          pushHistory();
        }, 400);
      }

      function afterHistoryLoad() {
        try {
          artW = (typeof canvas.getWidth === "function" ? canvas.getWidth() : canvas.width) || artW;
          artH = (typeof canvas.getHeight === "function" ? canvas.getHeight() : canvas.height) || artH;
          var el = document.getElementById("artboard");
          el.width = artW;
          el.height = artH;
          gridGroup = null;
          backGridGroup = null;
          canvas.forEachObject(function (o) {
            if (o.layerType === "grid") gridGroup = o;
            if (o.layerType === "gridBack") backGridGroup = o;
          });
          canvas.discardActiveObject();
          renderRulers();
          syncLayers();
          buildPresets();
          analyzeImages();
          safeRender();
        } finally {
          historyPaused = false;
          updateUndoButton();
        }
      }

      function doUndo() {
        if (!canvas || historyIndex <= 0 || historyPaused) return;
        historyPaused = true;
        historyIndex--;
        var raw = historyStack[historyIndex];
        var data = typeof raw === "string" ? JSON.parse(raw) : raw;
        try {
          function onLoadDone() {
            try {
              afterHistoryLoad();
            } catch (e) {
              console.warn(e);
              historyPaused = false;
              historyIndex++;
              updateUndoButton();
            }
          }
          if (canvas.loadFromJSON.length >= 2) {
            canvas.loadFromJSON(data, onLoadDone);
          } else {
            var ret = canvas.loadFromJSON(data);
            if (ret && typeof ret.then === "function") {
              ret.then(onLoadDone).catch(function (err) {
                console.warn(err);
                historyPaused = false;
                historyIndex++;
                updateUndoButton();
              });
            } else {
              onLoadDone();
            }
          }
        } catch (e) {
          console.warn(e);
          historyPaused = false;
          historyIndex++;
          updateUndoButton();
        }
      }

      function loadFabricImage(url) {
        var ret = FabricImage.fromURL(url, { crossOrigin: "anonymous" });
        if (ret && typeof ret.then === "function") return ret;
        return new Promise(function (res, rej) {
          try {
            FabricImage.fromURL(url, function (img) { img ? res(img) : rej(new Error("empty")); }, { crossOrigin: "anonymous" });
          } catch (err) { rej(err); }
        });
      }

      function fitArtboardSize(nw, nh) {
        if (!nw || !nh) return { w: DEFAULT_W, h: DEFAULT_H };
        var scale = Math.min(1, MAX_ART / Math.max(nw, nh));
        var w = Math.round(nw * scale);
        var h = Math.round(nh * scale);
        if (Math.min(w, h) < MIN_ART) {
          var up = MIN_ART / Math.min(w, h);
          w = Math.min(MAX_ART, Math.round(w * up));
          h = Math.min(MAX_ART, Math.round(h * up));
        }
        if (Math.max(w, h) > MAX_ART) {
          var r = MAX_ART / Math.max(w, h);
          w = Math.round(w * r);
          h = Math.round(h * r);
        }
        return { w: Math.max(200, w), h: Math.max(200, h) };
      }

      function scaleCanvasContent(oldW, oldH, newW, newH) {
        if (!canvas || oldW <= 0 || oldH <= 0) return;
        var sx = newW / oldW;
        var sy = newH / oldH;
        canvas.getObjects().forEach(function (obj) {
          if (isGuideLayer(obj)) return;
          try {
            obj.set({
              left: (obj.left || 0) * sx,
              top: (obj.top || 0) * sy,
              scaleX: (obj.scaleX || 1) * sx,
              scaleY: (obj.scaleY || 1) * sy
            });
            if (isText(obj) && obj.fontSize) {
              var u = Math.min(sx, sy);
              obj.set({
                fontSize: Math.max(8, Math.round(obj.fontSize * u)),
                width: Math.max(40, Math.round((obj.width || 300) * sx))
              });
            }
            obj.setCoords();
          } catch (e) { console.warn(e); }
        });
      }

      function resizeArtboard(newW, newH) {
        newW = Math.max(MIN_ART, Math.min(MAX_ART, Math.round(newW)));
        newH = Math.max(MIN_ART, Math.min(MAX_ART, Math.round(newH)));
        if (!canvas) {
          artW = newW;
          artH = newH;
          return;
        }
        var ow = artW;
        var oh = artH;
        if (ow === newW && oh === newH) {
          drawGrid(currentPresetId, getReferenceImage());
          renderRulers();
          scrollViewportToShowRulers();
          safeRender();
          return;
        }
        silentHistory++;
        try {
          scaleCanvasContent(ow, oh, newW, newH);
          artW = newW;
          artH = newH;
          canvas.setDimensions({ width: artW, height: artH });
          var el = document.getElementById("artboard");
          el.width = artW;
          el.height = artH;
          drawGrid(currentPresetId, getReferenceImage());
          renderRulers();
          scrollViewportToShowRulers();
          safeRender();
          buildPresets();
          console.log("Artboard resized", artW, artH);
        } finally {
          silentHistory--;
        }
        scheduleHistoryPush();
      }

      function installImageDeleteControl() {
        if (!Control) return;
        var proto = FabricImage.prototype;
        function delHandler(eventData, transform) {
          try {
            var t = transform.target;
            if (t && t.layerType === "image") {
              canvas.remove(t);
              canvas.discardActiveObject();
              safeRender();
              syncLayers();
            }
          } catch (e) {}
          return true;
        }
        function renderX(ctx, left, top, styleOverride, fo) {
          var sz = 22;
          ctx.save();
          var cx = left + fo.cornerSize / 2;
          var cy = top + fo.cornerSize / 2;
          ctx.beginPath();
          ctx.arc(cx, cy, sz / 2, 0, Math.PI * 2);
          ctx.fillStyle = "rgba(25,25,25,0.92)";
          ctx.fill();
          ctx.strokeStyle = "rgba(255,255,255,0.35)";
          ctx.stroke();
          ctx.strokeStyle = "rgba(255,255,255,0.9)";
          ctx.lineWidth = 1.35;
          ctx.beginPath();
          ctx.moveTo(cx - 5, cy - 5); ctx.lineTo(cx + 5, cy + 5);
          ctx.moveTo(cx + 5, cy - 5); ctx.lineTo(cx - 5, cy + 5);
          ctx.stroke();
          ctx.restore();
        }
        proto.controls = proto.controls || {};
        proto.controls.deleteCtrl = new Control({
          x: 0.5, y: -0.5, offsetY: -14, offsetX: 14,
          cursorStyle: "pointer", mouseUpHandler: delHandler, render: renderX, cornerSize: 28
        });
      }

      function isGuideLayer(o) {
        return o && (o.layerType === "grid" || o.layerType === "gridBack");
      }
      function isText(o) {
        var t = o && o.type;
        return t === "textbox" || t === "i-text" || t === "text";
      }
      function userObjects() {
        return canvas.getObjects().filter(function (o) { return !isGuideLayer(o); });
      }
      function syncGuideStackOrder() {
        try {
          if (backGridGroup && canvas.getObjects().indexOf(backGridGroup) >= 0 && canvas.sendObjectToBack) {
            canvas.sendObjectToBack(backGridGroup);
          }
          if (gridGroup && canvas.getObjects().indexOf(gridGroup) >= 0 && canvas.bringObjectToFront) {
            canvas.bringObjectToFront(gridGroup);
          }
        } catch (e) {}
      }
      function applyRulerChrome() {
        var shell = document.getElementById("canvasShell");
        if (!shell) return;
        if (rulersVisible) shell.classList.remove("rulers-hidden");
        else shell.classList.add("rulers-hidden");
      }

      function scrollViewportToShowRulers() {
        try {
          var vp = document.getElementById("viewport");
          if (vp) {
            vp.scrollTop = 0;
            vp.scrollLeft = 0;
          }
        } catch (e) {}
      }

      function renderRulers() {
        try {
          var rt = document.getElementById("rulerTop");
          var rl = document.getElementById("rulerLeft");
          if (!rt || !rl) return;
          rt.style.width = artW + "px";
          rl.style.height = artH + "px";
          rt.querySelectorAll(".ruler-tick-h").forEach(function (n) { n.remove(); });
          rl.querySelectorAll(".ruler-tick-v").forEach(function (n) { n.remove(); });
          if (!rulersVisible) return;
          var step = artW > 1400 ? 100 : artW > 700 ? 50 : 25;
          for (var x = 0; x <= artW; x += step) {
            var d = document.createElement("div");
            d.className = "ruler-tick-h";
            d.style.left = x + "px";
            d.style.height = x % (step * 2) === 0 ? "55%" : "35%";
            d.textContent = x === 0 ? "" : String(x);
            rt.appendChild(d);
          }
          for (var y = 0; y <= artH; y += step) {
            var dv = document.createElement("div");
            dv.className = "ruler-tick-v";
            dv.style.top = y + "px";
            dv.style.width = y % (step * 2) === 0 ? "55%" : "35%";
            dv.textContent = String(y);
            rl.appendChild(dv);
          }
        } catch (e) { console.warn(e); }
      }

      function isRasterLike(o) {
        if (!o) return false;
        if (o.layerType === "image") return true;
        if (o.type === "image") return true;
        return false;
      }

      function getReferenceImage() {
        var o = canvas && canvas.getActiveObject();
        if (o && isRasterLike(o)) return o;
        var imgs = canvas.getObjects().filter(isRasterLike);
        if (!imgs.length) return null;
        var best = null, maxA = 0;
        imgs.forEach(function (im) {
          im.setCoords();
          var b = im.getBoundingRect(true, true);
          var a = b.width * b.height;
          if (a >= maxA) { maxA = a; best = im; }
        });
        return best;
      }

      /** 單張圖片內取樣：亮度與視覺重心（0~1，相對於圖片範圍） */
      function analyzeImageLocal(img) {
        var def = { lum: 0.5, cx: 0.5, cy: 0.5, ar: analysis.ar, ag: analysis.ag, ab: analysis.ab };
        if (!img) return def;
        var el = img.getElement && img.getElement();
        if (!el || !el.complete || !el.naturalWidth) return def;
        try {
          pctx.drawImage(el, 0, 0, 48, 48);
          var d = pctx.getImageData(0, 0, 48, 48).data;
          var sumL = 0, cnt = 0, sx = 0, sy = 0, sw = 0, tr = 0, tg = 0, tb = 0;
          for (var i = 0; i < d.length; i += 4) {
            var r = d[i], g = d[i + 1], b = d[i + 2];
            var L = (0.2126 * r + 0.7152 * g + 0.0722 * b) / 255;
            var w = 0.12 + Math.abs(L - 0.5);
            var xi = (i / 4) % 48, yi = Math.floor((i / 4) / 48);
            sumL += L; cnt++;
            sx += xi * w; sy += yi * w; sw += w;
            tr += r; tg += g; tb += b;
          }
          if (!cnt) return def;
          return {
            lum: sumL / cnt,
            cx: sw ? sx / sw / 48 : 0.5,
            cy: sw ? sy / sw / 48 : 0.5,
            ar: Math.round(tr / cnt),
            ag: Math.round(tg / cnt),
            ab: Math.round(tb / cnt)
          };
        } catch (e) {
          return def;
        }
      }

      /** 擴大畫布（不縮放物件）以包住所有圖片 */
      function growArtboardToFitAllImages() {
        try {
          var imgs = canvas.getObjects().filter(isRasterLike);
          if (!imgs.length) return;
          var pad = 36;
          var minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity;
          imgs.forEach(function (im) {
            im.setCoords();
            var b = im.getBoundingRect(true, true);
            minX = Math.min(minX, b.left);
            minY = Math.min(minY, b.top);
            maxX = Math.max(maxX, b.left + b.width);
            maxY = Math.max(maxY, b.top + b.height);
          });
          var nw = Math.min(MAX_ART, Math.max(artW, Math.ceil(maxX + pad)));
          var nh = Math.min(MAX_ART, Math.max(artH, Math.ceil(maxY + pad)));
          if (nw <= artW && nh <= artH) return;
          silentHistory++;
          try {
            artW = nw;
            artH = nh;
            canvas.setDimensions({ width: artW, height: artH });
            var el = document.getElementById("artboard");
            el.width = artW;
            el.height = artH;
            drawGrid(currentPresetId, getReferenceImage());
            renderRulers();
            scrollViewportToShowRulers();
            buildPresets();
            safeRender();
            console.log("Artboard grown to fit images", artW, artH);
          } finally {
            silentHistory--;
          }
          pushHistory();
        } catch (e) { console.warn(e); }
      }

      var redrawGridTimer = null;
      function scheduleGridRedraw() {
        clearTimeout(redrawGridTimer);
        redrawGridTimer = setTimeout(function () {
          try {
            drawGrid(currentPresetId, getReferenceImage());
            safeRender();
          } catch (e) {}
        }, 120);
      }

      /**
       * 依網格預設在矩形 (ox,oy,W,H) 內產線段物件（畫布絕對 px）。
       */
      function makePresetLineObjects(pid, ox, oy, W, H, stroke, sw) {
        var lines = [];
        var strokeW = sw == null ? 1 : sw;
        function ln(a, b, c, d) {
          lines.push(new Line([a, b, c, d], {
            stroke: stroke,
            strokeWidth: strokeW,
            selectable: false,
            evented: false
          }));
        }
        var mEdge = Math.max(16, Math.round(Math.min(W, H) * 0.045));
        if (pid === "thirds") {
          ln(ox + W / 3, oy, ox + W / 3, oy + H);
          ln(ox + (W * 2) / 3, oy, ox + (W * 2) / 3, oy + H);
          ln(ox, oy + H / 3, ox + W, oy + H / 3);
          ln(ox, oy + (H * 2) / 3, ox + W, oy + (H * 2) / 3);
        } else if (pid === "golden") {
          var phi = 0.6180339887;
          ln(ox + W * phi, oy, ox + W * phi, oy + H);
          ln(ox + W * (1 - phi), oy, ox + W * (1 - phi), oy + H);
          ln(ox, oy + H * phi, ox + W, oy + H * phi);
          ln(ox, oy + H * (1 - phi), ox + W, oy + H * (1 - phi));
        } else if (pid === "center") {
          ln(ox + W / 2, oy, ox + W / 2, oy + H);
          ln(ox, oy + H / 2, ox + W, oy + H / 2);
        } else if (pid === "asym") {
          ln(ox + W * 0.36, oy, ox + W * 0.36, oy + H);
          ln(ox, oy + H * 0.44, ox + W, oy + H * 0.44);
        } else if (pid === "modular") {
          var cols = 12, g = Math.max(4, Math.round(W * 0.012));
          var cwM = (W - g * 11) / 12, xm = ox;
          for (var im = 0; im <= cols; im++) {
            ln(xm, oy, xm, oy + H);
            xm += cwM + (im < cols ? g : 0);
          }
          ln(ox, oy + H * 0.34, ox + W, oy + H * 0.34);
          ln(ox, oy + H * 0.7, ox + W, oy + H * 0.7);
        } else if (pid === "axis") {
          ln(ox + W / 2, oy, ox + W / 2, oy + H);
          ln(ox, oy + H * 0.62, ox + W, oy + H * 0.62);
          ln(ox + W * 0.18, oy, ox + W * 0.18, oy + H);
          ln(ox + W * 0.82, oy, ox + W * 0.82, oy + H);
          ln(ox, oy + H * 0.34, ox + W, oy + H * 0.34);
        } else if (pid === "radial") {
          var rcx = ox + W / 2, rcy = oy + H / 2;
          var targets = [
            [ox, oy], [ox + W, oy], [ox + W, oy + H], [ox, oy + H],
            [ox + W / 2, oy], [ox + W / 2, oy + H], [ox, oy + H / 2], [ox + W, oy + H / 2]
          ];
          for (var ri = 0; ri < targets.length; ri++) {
            ln(rcx, rcy, targets[ri][0], targets[ri][1]);
          }
        } else if (pid === "random_grid") {
          function rg01(i) {
            var sr = Math.sin((i + 1) * 12.9898 + W * 0.001 + H * 0.001 + ox * 0.01 + oy * 0.01) * 43758.5453;
            return sr - Math.floor(sr);
          }
          var rn = 11;
          for (var rgi = 0; rgi < rn; rgi++) {
            if (rg01(rgi) < 0.55) {
              var px = ox + rg01(rgi + 17) * W;
              ln(px, oy, px, oy + H);
            } else {
              var py = oy + rg01(rgi + 29) * H;
              ln(ox, py, ox + W, py);
            }
          }
          for (var rd = 0; rd < 4; rd++) {
            var t0 = rg01(rd + 40);
            ln(ox, oy + t0 * H, ox + W, oy + (1 - t0) * H);
          }
        } else if (pid === "marching") {
          var bands = 5;
          for (var mb = 0; mb <= bands; mb++) {
            ln(ox, oy + (mb / bands) * H, ox + W, oy + (mb / bands) * H);
          }
          var mcols = 4;
          for (var mc = 0; mc <= mcols; mc++) {
            var bx = ox + (mc / mcols) * W;
            for (var mb2 = 0; mb2 < bands; mb2++) {
              var mshift = (mb2 % 2) * (W / mcols * 0.22);
              ln(bx + mshift, oy + (mb2 / bands) * H, bx + mshift, oy + ((mb2 + 1) / bands) * H);
            }
          }
        } else if (pid === "mod_square") {
          var ux = 8, uy = 6;
          var sg = Math.max(3, Math.round(Math.min(W, H) * 0.014));
          var cw = (W - sg * (ux - 1)) / ux;
          var ch = (H - sg * (uy - 1)) / uy;
          var ci2, cj2;
          for (ci2 = 0; ci2 <= ux; ci2++) {
            var xv = ci2 < ux ? ox + ci2 * (cw + sg) : ox + ux * cw + (ux - 1) * sg;
            ln(xv, oy, xv, oy + H);
          }
          for (cj2 = 0; cj2 <= uy; cj2++) {
            var yv = cj2 < uy ? oy + cj2 * (ch + sg) : oy + uy * ch + (uy - 1) * sg;
            ln(ox, yv, ox + W, yv);
          }
        } else if (pid === "bilateral") {
          var midx = ox + W / 2;
          ln(midx, oy, midx, oy + H);
          [0.15, 0.28, 0.38].forEach(function (t) {
            ln(ox + W * t, oy, ox + W * t, oy + H);
            ln(ox + W * (1 - t), oy, ox + W * (1 - t), oy + H);
          });
          ln(ox, oy + H * 0.4, ox + W, oy + H * 0.4);
          ln(ox, oy + H * 0.6, ox + W, oy + H * 0.6);
        } else {
          var m = mEdge;
          ln(ox + m, oy + m, ox + W - m, oy + m);
          ln(ox + m, oy + H - m, ox + W - m, oy + H - m);
          ln(ox + m, oy + m, ox + m, oy + H - m);
          ln(ox + W - m, oy + m, ox + W - m, oy + H - m);
        }
        return lines;
      }

      /**
       * 背景淡線（整張畫布）＋置頂對齊線（參考圖框內）；顏色近似 Illustrator 參考線。
       */
      function drawGrid(presetId, refImg) {
        silentHistory++;
        try {
          currentPresetId = presetId || currentPresetId;
          var pid = currentPresetId;
          if (gridGroup) {
            canvas.remove(gridGroup);
            gridGroup = null;
          }
          if (backGridGroup) {
            canvas.remove(backGridGroup);
            backGridGroup = null;
          }
          var ox = 0, oy = 0, W = artW, H = artH;
          if (refImg) {
            refImg.setCoords();
            var br = refImg.getBoundingRect(true, true);
            if (br.width > 4 && br.height > 4) {
              ox = br.left;
              oy = br.top;
              W = br.width;
              H = br.height;
            }
          }
          var pal = GUIDE_PALETTES[guidePaletteKey] || GUIDE_PALETTES.mint;
          var objsBack = makePresetLineObjects(pid, 0, 0, artW, artH, pal.back, 1);
          var objsFg = makePresetLineObjects(pid, ox, oy, W, H, pal.front, 1);
          backGridGroup = new Group(objsBack, { selectable: false, evented: false, hasControls: false });
          backGridGroup.layerType = "gridBack";
          gridGroup = new Group(objsFg, { selectable: false, evented: false, hasControls: false });
          gridGroup.layerType = "grid";
          canvas.add(backGridGroup);
          canvas.add(gridGroup);
          backGridGroup.visible = alignGridVisible;
          gridGroup.visible = alignGridVisible;
          syncGuideStackOrder();
          safeRender();
        } catch (e) { console.warn("drawGrid", e); }
        finally {
          silentHistory--;
        }
      }

      function analyzeImages() {
        try {
          var imgs = canvas.getObjects().filter(function (o) {
            return o.layerType === "image" || o.type === "image";
          });
          if (!imgs.length) {
            analysis = { lum: 0.5, cx: 0.5, cy: 0.5, ar: 210, ag: 210, ab: 210 };
            buildPresets();
            return;
          }
          var sumL = 0, cnt = 0, sx = 0, sy = 0, sw = 0, tr = 0, tg = 0, tb = 0;
          imgs.forEach(function (im) {
            var el = im.getElement && im.getElement();
            if (!el || !el.complete) return;
            try {
              pctx.drawImage(el, 0, 0, 48, 48);
              var d = pctx.getImageData(0, 0, 48, 48).data;
              for (var i = 0; i < d.length; i += 4) {
                var r = d[i], g = d[i + 1], b = d[i + 2];
                var L = (0.2126 * r + 0.7152 * g + 0.0722 * b) / 255;
                var w = 0.12 + Math.abs(L - 0.5);
                var xi = (i / 4) % 48, yi = Math.floor((i / 4) / 48);
                sumL += L; cnt++;
                sx += xi * w; sy += yi * w; sw += w;
                tr += r; tg += g; tb += b;
              }
            } catch (e) {}
          });
          if (!cnt) { analysis = { lum: 0.5, cx: 0.5, cy: 0.5, ar: 200, ag: 200, ab: 200 }; buildPresets(); return; }
          analysis.lum = sumL / cnt;
          analysis.cx = sw ? sx / sw / 48 : 0.5;
          analysis.cy = sw ? sy / sw / 48 : 0.5;
          var px = cnt / 4;
          analysis.ar = Math.round(tr / Math.max(px, 1));
          analysis.ag = Math.round(tg / Math.max(px, 1));
          analysis.ab = Math.round(tb / Math.max(px, 1));
          buildPresets();
        } catch (e) {
          buildPresets();
        }
      }

      function pickColorFromLoc(loc) {
        var lum = loc.lum;
        var cont = lum > 0.52 ? { r: 22, g: 22, b: 24 } : { r: 252, g: 252, b: 250 };
        var har = {
          r: Math.max(0, Math.min(255, 255 - loc.ar)),
          g: Math.max(0, Math.min(255, 255 - loc.ag)),
          b: Math.max(0, Math.min(255, 255 - loc.ab))
        };
        var m = 0.5;
        return {
          r: Math.round(cont.r * m + har.r * (1 - m)),
          g: Math.round(cont.g * m + har.g * (1 - m)),
          b: Math.round(cont.b * m + har.b * (1 - m))
        };
      }

      function distribute01(i, seed) {
        var s = Math.sin((i + 1) * 12.9898 + (seed >>> 0) * 0.0000123) * 43758.5453;
        return s - Math.floor(s);
      }

      function layoutSeed(p, refImg) {
        var base = (artW | 0) + (artH | 0) * 1.713 + (p.id ? p.id.length * 997 : 0);
        if (refImg && refImg.layerId) {
          var sid = refImg.layerId;
          for (var si = 0; si < sid.length; si++) {
            base = ((base << 5) - base) + sid.charCodeAt(si);
            base |= 0;
          }
        }
        return (base >>> 0) + 24613;
      }

      /** 無圖片時：相對整張畫布 */
      function layoutForArtboard(p) {
        var W = artW, H = artH;
        var c = analysis;
        var rgb = pickColorFromLoc({ lum: c.lum, cx: c.cx, cy: c.cy, ar: c.ar, ag: c.ag, ab: c.ab });
        var fsMul = p.id === "center" || p.id === "bilateral" || p.id === "radial" ? 1.15 : p.id === "edge" ? 0.85 : 1;
        var fs = Math.round(26 + Math.min(W, H) / 25 * fsMul);
        var font = FONTS[(p.id.length + Math.floor(c.lum * 8)) % FONTS.length];
        var pad = Math.max(12, Math.round(Math.min(W, H) * 0.04));
        var x, y, ox, oy, w, align;
        var sd;
        if (p.id === "thirds") {
          x = c.cx > 0.52 ? pad : W * 0.54; y = pad + Math.round(H * 0.04);
          ox = "left"; oy = "top"; w = W * 0.38; align = "left";
        } else if (p.id === "golden") {
          x = W * 0.54; y = pad + Math.round(H * 0.03); ox = "left"; oy = "top"; w = W * 0.36; align = "left";
        } else if (p.id === "center") {
          x = W / 2; y = H * 0.44; ox = "center"; oy = "center"; w = W * 0.55; align = "center";
        } else if (p.id === "asym") {
          x = c.cx < 0.48 ? W * 0.54 : pad; y = H * 0.16; ox = "left"; oy = "top"; w = W * 0.4; align = "left";
        } else if (p.id === "modular") {
          x = pad; y = pad + 8; ox = "left"; oy = "top"; w = W - pad * 2; align = "left";
        } else if (p.id === "axis") {
          x = pad + Math.round(W * 0.02); y = pad + Math.round(H * 0.08);
          ox = "left"; oy = "top"; w = W * 0.36; align = "left";
        } else if (p.id === "radial") {
          x = W / 2; y = H * 0.4; ox = "center"; oy = "center"; w = W * 0.52; align = "center";
        } else if (p.id === "random_grid") {
          sd = layoutSeed(p, null);
          x = W * (0.14 + distribute01(1, sd) * 0.48);
          y = H * (0.1 + distribute01(2, sd) * 0.38);
          ox = "left"; oy = "top"; w = W * (0.26 + distribute01(3, sd) * 0.24); align = "left";
        } else if (p.id === "marching") {
          x = pad + Math.round(W * 0.04); y = pad + Math.round(H * 0.1);
          ox = "left"; oy = "top"; w = W * 0.4; align = "left";
        } else if (p.id === "mod_square") {
          x = pad; y = pad + 8; ox = "left"; oy = "top"; w = W - pad * 2; align = "left";
        } else if (p.id === "bilateral") {
          x = W / 2; y = H * 0.42; ox = "center"; oy = "center"; w = W * 0.5; align = "center";
        } else {
          var edgePad = Math.max(24, Math.round(Math.min(W, H) * 0.045));
          x = edgePad; y = edgePad; ox = "left"; oy = "top"; w = W - edgePad * 2; align = "left";
        }
        return {
          x: x, y: y, originX: ox, originY: oy, width: w, textAlign: align,
          fontSize: fs, fontFamily: font, rgb: rgb,
          fill: "rgb(" + rgb.r + "," + rgb.g + "," + rgb.b + ")"
        };
      }

      /** 以參考圖片邊界框為排版區域；座標為畫布絕對 px */
      function layoutFor(p, refImg) {
        if (!refImg) return layoutForArtboard(p);
        refImg.setCoords();
        var r = refImg.getBoundingRect(true, true);
        var L = r.left, T = r.top, W = r.width, H = r.height;
        if (W < 8 || H < 8) return layoutForArtboard(p);
        var loc = analyzeImageLocal(refImg);
        var rgb = pickColorFromLoc(loc);
        var fsMul2 = p.id === "center" || p.id === "bilateral" || p.id === "radial" ? 1.15 : p.id === "edge" ? 0.85 : 1;
        var fs = Math.round(18 + Math.min(W, H) / 20 * fsMul2);
        var font = FONTS[(p.id.length + Math.floor(loc.lum * 8)) % FONTS.length];
        var pad = Math.max(8, Math.round(Math.min(W, H) * 0.04));
        var c = loc;
        var x, y, ox, oy, w, align;
        var sd2;
        if (p.id === "thirds") {
          x = L + (c.cx > 0.52 ? pad : W * 0.52);
          y = T + pad + Math.round(H * 0.04);
          ox = "left"; oy = "top"; w = W * 0.38; align = "left";
        } else if (p.id === "golden") {
          x = L + W * 0.52; y = T + pad + Math.round(H * 0.03);
          ox = "left"; oy = "top"; w = W * 0.36; align = "left";
        } else if (p.id === "center") {
          x = L + W / 2; y = T + H * 0.44; ox = "center"; oy = "center"; w = W * 0.55; align = "center";
        } else if (p.id === "asym") {
          x = L + (c.cx < 0.48 ? W * 0.52 : pad); y = T + H * 0.16;
          ox = "left"; oy = "top"; w = W * 0.4; align = "left";
        } else if (p.id === "modular") {
          x = L + pad; y = T + pad + 8; ox = "left"; oy = "top"; w = W - pad * 2; align = "left";
        } else if (p.id === "axis") {
          x = L + Math.round(W * 0.06); y = T + Math.round(H * 0.1);
          ox = "left"; oy = "top"; w = W * 0.34; align = "left";
        } else if (p.id === "radial") {
          x = L + W / 2; y = T + H * 0.38; ox = "center"; oy = "center"; w = W * 0.52; align = "center";
        } else if (p.id === "random_grid") {
          sd2 = layoutSeed(p, refImg);
          x = L + W * (0.12 + distribute01(1, sd2) * 0.52);
          y = T + H * (0.1 + distribute01(2, sd2) * 0.4);
          ox = "left"; oy = "top"; w = W * (0.26 + distribute01(3, sd2) * 0.22); align = "left";
        } else if (p.id === "marching") {
          x = L + Math.round(W * 0.06); y = T + Math.round(H * 0.11);
          ox = "left"; oy = "top"; w = W * 0.38; align = "left";
        } else if (p.id === "mod_square") {
          x = L + pad; y = T + pad + 6; ox = "left"; oy = "top"; w = W - pad * 2; align = "left";
        } else if (p.id === "bilateral") {
          x = L + W / 2; y = T + H * 0.42; ox = "center"; oy = "center"; w = W * 0.48; align = "center";
        } else {
          var edgePad = Math.max(12, Math.round(Math.min(W, H) * 0.045));
          x = L + edgePad; y = T + edgePad; ox = "left"; oy = "top"; w = W - edgePad * 2; align = "left";
        }
        return {
          x: x, y: y, originX: ox, originY: oy, width: w, textAlign: align,
          fontSize: fs, fontFamily: font, rgb: rgb,
          fill: "rgb(" + rgb.r + "," + rgb.g + "," + rgb.b + ")"
        };
      }

      function buildPresets() {
        var host = document.getElementById("presetHost");
        host.innerHTML = "";
        var ref = getReferenceImage();
        var refNote = ref
          ? (I18N[currentLang].presetScopeWithRef((ref.layerLabel || "Image")))
          : I18N[currentLang].presetScopeNoRef;
        var hint = document.createElement("div");
        hint.className = "text-[10px] text-[#A0A0A0] mb-2 leading-relaxed px-0.5";
        hint.innerHTML = refNote;
        host.appendChild(hint);

        PRESETS.forEach(function (pr) {
          var lay = layoutFor(pr, ref);
          var card = document.createElement("div");
          card.className = "rounded-xl border border-white/10 bg-white/[0.04] hover:bg-white/[0.07] p-3 cursor-pointer transition text-xs";
          card.innerHTML =
            '<div class="font-semibold text-[13px] text-white/95">' + presetTitle(pr) + '</div>' +
            '<div class="text-[10px] text-[#A0A0A0] mt-0.5">' + presetSubtitle(pr) + '</div>' +
            '<div class="mt-2 grid grid-cols-2 gap-1 text-[10px] text-[#A0A0A0]">' +
            '<span>' + t("presetMetaFont") + '</span><span class="text-white/80 truncate">' + lay.fontFamily + '</span>' +
            '<span>' + t("presetMetaSize") + '</span><span class="text-white/80">' + lay.fontSize + 'px</span>' +
            '<span>' + t("presetMetaRgb") + '</span><span class="text-white/80">' + lay.rgb.r + "," + lay.rgb.g + "," + lay.rgb.b + '</span>' +
            '</div>' +
            '<div class="mt-1 text-[10px] text-[#A0A0A0]">' + lay.textAlign + ' · (' + Math.round(lay.x) + "," + Math.round(lay.y) + ")</div>" +
            '<div class="mt-2 text-[10px] text-white/40">' + t("presetApplyHint") + '</div>';
          card.addEventListener("click", function () {
            try {
              var imgRef = getReferenceImage();
              if (!imgRef) {
                showToast(t("toastNeedImage"));
                return;
              }
              var freshLay = layoutFor(pr, imgRef);
              drawGrid(pr.id, imgRef);
              applyLayout(freshLay, pr);
            } catch (e) { console.warn(e); }
          });
          host.appendChild(card);
        });
      }

      function edgeCharSpacingPx() {
        var im = getReferenceImage();
        if (im) {
          im.setCoords();
          var b = im.getBoundingRect(true, true);
          return Math.round(Math.min(b.width, b.height) * 0.04);
        }
        return Math.round(Math.min(artW, artH) * 0.04);
      }

      function createNewTextFromLayout(lay, pr) {
        var sample = pr && (pr.id === "center" || pr.id === "bilateral" || pr.id === "radial")
          ? "CENTERED\nHEADLINE"
          : "Editorial Title";
        var tb = new Textbox(sample, {
          left: lay.x, top: lay.y, originX: lay.originX, originY: lay.originY,
          width: lay.width, fontSize: lay.fontSize,
          fontFamily: lay.fontFamily + ", " + FONTS.join(", "),
          fill: lay.fill, textAlign: lay.textAlign, fontWeight: "600",
          lineHeight: 1.15, editable: true, cornerColor: "#fff", borderColor: "rgba(255,255,255,0.35)",
          charSpacing: pr && pr.id === "edge" ? edgeCharSpacingPx() : 0,
          lockMovementX: true,
          lockMovementY: true
        });
        tb.layerId = genId();
        tb.layerType = "text";
        tb.typographyLocked = true;
        canvas.add(tb);
        canvas.setActiveObject(tb);
        syncGuideStackOrder();
        safeRender();
        syncLayers();
        return tb;
      }

      function addBlankTextLayer() {
        var tb = new Textbox(currentLang === "en" ? "New text\n(Double-click to edit)" : "新增文字\n（雙擊編輯）", {
          left: artW / 2, top: artH / 2, originX: "center", originY: "center",
          width: Math.min(420, artW * 0.45), fontSize: Math.round(28 + Math.min(artW, artH) / 40),
          fontFamily: FONTS[0] + ", " + FONTS.join(", "),
          fill: "rgba(255,255,255,0.92)", textAlign: "center", fontWeight: "600", lineHeight: 1.2,
          editable: true, cornerColor: "#fff", borderColor: "rgba(255,255,255,0.35)",
          shadow: "0 2px 12px rgba(0,0,0,0.35)",
          lockMovementX: false,
          lockMovementY: false
        });
        tb.layerId = genId();
        tb.layerType = "text";
        tb.typographyLocked = false;
        canvas.add(tb);
        canvas.setActiveObject(tb);
        syncGuideStackOrder();
        safeRender();
        syncLayers();
        refreshProps();
      }

      function applyLayout(lay, pr) {
        var o = canvas.getActiveObject();
        var tb = (o && isText(o)) ? o : createNewTextFromLayout(lay, pr);
        tb.set({
          left: lay.x, top: lay.y, originX: lay.originX, originY: lay.originY,
          width: lay.width, fontSize: lay.fontSize,
          fontFamily: lay.fontFamily + ", " + FONTS.join(", "),
          fill: lay.fill, textAlign: lay.textAlign,
          charSpacing: pr && pr.id === "edge" ? edgeCharSpacingPx() : 0,
          lockMovementX: true,
          lockMovementY: true
        });
        tb.typographyLocked = true;
        tb.setCoords();
        canvas.setActiveObject(tb);
        syncGuideStackOrder();
        safeRender();
        syncLayers();
        refreshProps();
      }

      function reorderStack(fromId, toId) {
        try {
          var list = userObjects();
          if (list.length < 2) return;
          var ui = list.slice().reverse();
          var i0 = ui.findIndex(function (o) { return o.layerId === fromId; });
          var i1 = ui.findIndex(function (o) { return o.layerId === toId; });
          if (i0 < 0 || i1 < 0 || i0 === i1) return;
          var item = ui.splice(i0, 1)[0];
          ui.splice(i1, 0, item);
          var bottomToTop = ui.slice().reverse();
          bottomToTop.forEach(function (o) { try { canvas.remove(o); } catch (e) {} });
          bottomToTop.forEach(function (o) { canvas.add(o); });
          syncGuideStackOrder();
          safeRender();
          syncLayers();
        } catch (e) { console.warn(e); }
      }

      function syncLayers() {
        try {
          var listEl = document.getElementById("layerList");
          listEl.innerHTML = "";
          var uiOrder = userObjects().slice().reverse();
          var act = canvas.getActiveObject();
          uiOrder.forEach(function (obj) {
            if (!obj.layerId) obj.layerId = genId();
            if (!obj.layerType) obj.layerType = obj.type === "image" ? "image" : isText(obj) ? "text" : "other";

            var row = document.createElement("div");
            row.className = "layer-row flex items-center gap-2 rounded-lg px-2 py-1.5 hover:bg-white/[0.06] border border-transparent cursor-pointer";
            row.draggable = true;
            row.dataset.layerId = obj.layerId;

            var isAct = act === obj || (act && act.type === "activeSelection" && act.getObjects && act.getObjects().indexOf(obj) >= 0);
            if (isAct) row.classList.add("bg-white/[0.08]", "border-white/10");

            var thumb = document.createElement("div");
            thumb.className = "w-9 h-9 rounded-md bg-[#1E1E1E] border border-white/10 shrink-0 overflow-hidden flex items-center justify-center text-[10px] text-[#A0A0A0]";
            if (obj.type === "image" || obj.layerType === "image") {
              try {
                var u = obj.toDataURL({ format: "png", multiplier: 0.05 });
                thumb.style.backgroundImage = "url(" + u + ")";
                thumb.style.backgroundSize = "cover";
                thumb.textContent = "";
              } catch (e) { thumb.textContent = "IMG"; }
            } else if (obj.type === "group") {
              thumb.textContent = "▦";
            } else thumb.textContent = "T";

            var label = document.createElement("div");
            label.className = "flex-1 min-w-0";
            var title = obj.layerType === "image" ? (obj.layerLabel || "Image") :
              ((obj.text || "").replace(/\s+/g, " ").trim().slice(0, 34) || "Text");
            if (title.length > 32) title += "…";
            label.innerHTML = '<div class="text-[11px] font-medium truncate text-white/90">' + title + '</div>' +
              '<div class="text-[9px] text-[#A0A0A0]">' + (obj.layerType === "image" ? "Image" : "Type") + '</div>';

            var eye = document.createElement("button");
            eye.type = "button";
            eye.className = "w-7 h-7 rounded-md border border-white/10 bg-white/5 text-[11px] text-[#A0A0A0] hover:text-white shrink-0";
            eye.textContent = obj.visible === false ? "○" : "●";
            eye.addEventListener("click", function (e) {
              e.stopPropagation();
              obj.visible = obj.visible !== false ? false : true;
              safeRender();
              eye.textContent = obj.visible === false ? "○" : "●";
            });

            var del = document.createElement("button");
            del.type = "button";
            del.className = "w-7 h-7 rounded-md border border-white/10 bg-white/5 text-[#A0A0A0] hover:text-red-300 shrink-0 text-sm";
            del.innerHTML = "×";
            del.addEventListener("click", function (e) {
              e.stopPropagation();
              canvas.remove(obj);
              canvas.discardActiveObject();
              safeRender();
              syncLayers();
            });

            row.appendChild(thumb);
            row.appendChild(label);
            row.appendChild(eye);
            row.appendChild(del);

            row.addEventListener("click", function (e) {
              if (e.target.closest("button")) return;
              canvas.setActiveObject(obj);
              safeRender();
              syncLayers();
              refreshProps();
            });

            row.addEventListener("dragstart", function (e) {
              e.dataTransfer.setData("text/plain", obj.layerId);
              row.classList.add("opacity-50");
            });
            row.addEventListener("dragend", function () {
              row.classList.remove("opacity-50");
              listEl.querySelectorAll(".layer-row").forEach(function (n) { n.classList.remove("drag-target"); });
            });
            row.addEventListener("dragover", function (e) { e.preventDefault(); row.classList.add("drag-target"); });
            row.addEventListener("dragleave", function () { row.classList.remove("drag-target"); });
            row.addEventListener("drop", function (e) {
              e.preventDefault();
              row.classList.remove("drag-target");
              var id = e.dataTransfer.getData("text/plain");
              if (id && id !== obj.layerId) reorderStack(id, obj.layerId);
            });

            listEl.appendChild(row);
          });
          console.log("Layer synced");
        } catch (e) { console.warn(e); }
      }

      function fillFontSelect() {
        var sel = document.getElementById("pfFont");
        sel.innerHTML = "";
        FONTS.forEach(function (f) {
          var o = document.createElement("option");
          o.value = f; o.textContent = f;
          sel.appendChild(o);
        });
      }

      function setPropEnabled(on) {
        var p = document.getElementById("propPanel");
        p.classList.toggle("opacity-40", !on);
        p.classList.toggle("pointer-events-none", !on);
      }

      function refreshProps() {
        var o = canvas.getActiveObject();
        if (!o || !isText(o)) {
          setPropEnabled(false);
          document.getElementById("typoLockRow").classList.add("hidden");
          return;
        }
        setPropEnabled(true);
        var ff = (o.fontFamily || "").split(",")[0].trim().replace(/['"]/g, "");
        var sel = document.getElementById("pfFont");
        sel.value = FONTS.indexOf(ff) >= 0 ? ff : FONTS[0];
        document.getElementById("pfSizeR").value = Math.round(o.fontSize || 32);
        document.getElementById("pfSizeN").value = Math.round(o.fontSize || 32);
        var rgb = { r: 255, g: 255, b: 255 };
        var fill = o.fill || "#ffffff";
        if (typeof fill === "string" && fill.startsWith("#") && fill.length >= 7) {
          rgb.r = parseInt(fill.slice(1, 3), 16);
          rgb.g = parseInt(fill.slice(3, 5), 16);
          rgb.b = parseInt(fill.slice(5, 7), 16);
        }
        document.getElementById("pfR").value = rgb.r;
        document.getElementById("pfG").value = rgb.g;
        document.getElementById("pfB").value = rgb.b;
        document.getElementById("pfHex").value = "#" + [rgb.r, rgb.g, rgb.b].map(function (n) {
          return ("0" + Math.max(0, Math.min(255, n | 0)).toString(16)).slice(-2);
        }).join("");
        document.getElementById("pfOp").value = Math.round((typeof o.opacity === "number" ? o.opacity : 1) * 100);
        document.getElementById("pfLH").value = Math.round((o.lineHeight || 1.2) * 100);
        document.getElementById("pfCh").value = typeof o.charSpacing === "number" ? Math.round(o.charSpacing / 10) : 0;

        var lockRow = document.getElementById("typoLockRow");
        var unlockCb = document.getElementById("pfUnlockPos");
        if (o.typographyLocked) {
          lockRow.classList.remove("hidden");
          unlockCb.checked = !(o.lockMovementX && o.lockMovementY);
        } else {
          lockRow.classList.add("hidden");
        }
      }

      function applyPropsFromUI() {
        var o = canvas.getActiveObject();
        if (!o || !isText(o)) return;
        try {
          o.set({
            fontFamily: document.getElementById("pfFont").value + ", " + FONTS.join(", "),
            fontSize: parseInt(document.getElementById("pfSizeN").value, 10) || 32,
            fill: "rgb(" +
              Math.max(0, Math.min(255, parseInt(document.getElementById("pfR").value, 10) || 0)) + "," +
              Math.max(0, Math.min(255, parseInt(document.getElementById("pfG").value, 10) || 0)) + "," +
              Math.max(0, Math.min(255, parseInt(document.getElementById("pfB").value, 10) || 0)) + ")",
            opacity: (parseInt(document.getElementById("pfOp").value, 10) || 100) / 100,
            lineHeight: (parseInt(document.getElementById("pfLH").value, 10) || 120) / 100,
            charSpacing: (parseInt(document.getElementById("pfCh").value, 10) || 0) * 10
          });
          o.setCoords();
          safeRender();
          syncLayers();
        } catch (e) { console.warn(e); }
      }

      function initCanvas() {
        var el = document.getElementById("artboard");
        canvas = new Canvas(el, {
          width: artW,
          height: artH,
          backgroundColor: "#0c0c0c",
          preserveObjectStacking: true
        });
        installImageDeleteControl();
        drawGrid("thirds", null);
        renderRulers();
        applyRulerChrome();

        canvas.on("mouse:wheel", function (opt) {
          try {
            var e = opt.e;
            var z = canvas.getZoom() * Math.pow(0.999, e.deltaY);
            if (z > 4) z = 4;
            if (z < 0.12) z = 0.12;
            var pt = F.Point ? new F.Point(e.offsetX, e.offsetY) : { x: e.offsetX, y: e.offsetY };
            canvas.zoomToPoint(pt, z);
            e.preventDefault();
            e.stopPropagation();
            safeRender();
          } catch (err) { console.warn(err); }
        });

        canvas.on("mouse:down", function (opt) {
          var ev = opt.e;
          if (ev.button === 1 || (spaceDown && !canvas.getActiveObject())) {
            isPan = true;
            canvas.selection = false;
            lastPt = { x: ev.clientX, y: ev.clientY };
            ev.preventDefault();
          }
        });
        canvas.on("mouse:move", function (opt) {
          if (!isPan) return;
          var ev = opt.e;
          var vpt = canvas.viewportTransform;
          vpt[4] += ev.clientX - lastPt.x;
          vpt[5] += ev.clientY - lastPt.y;
          lastPt = { x: ev.clientX, y: ev.clientY };
          safeRender();
        });
        canvas.on("mouse:up", function () {
          isPan = false;
          canvas.selection = true;
        });

        canvas.on("mouse:dblclick", function (opt) {
          var t = opt.target;
          if (t && isText(t) && t.enterEditing) {
            t.enterEditing();
            if (t.selectAll) t.selectAll();
          }
        });

        var dSync = 0;
        function debSync() { clearTimeout(dSync); dSync = setTimeout(syncLayers, 50); }
        var dAna = 0;
        function debAna() { clearTimeout(dAna); dAna = setTimeout(analyzeImages, 120); }

        canvas.on("object:added", function (opt) {
          if (opt.target && isGuideLayer(opt.target)) return;
          debSync();
          debAna();
          scheduleHistoryPush();
          syncGuideStackOrder();
        });
        canvas.on("object:removed", function (opt) {
          if (opt.target && isGuideLayer(opt.target)) return;
          debSync();
          debAna();
          scheduleHistoryPush();
        });
        canvas.on("object:modified", function (opt) {
          if (opt.target && isGuideLayer(opt.target)) return;
          debSync();
          scheduleGridRedraw();
          scheduleHistoryPush();
          syncGuideStackOrder();
        });
        canvas.on("text:changed", function () {
          debAna();
          scheduleHistoryPush();
        });
        canvas.on("selection:created", function () { syncLayers(); refreshProps(); scheduleGridRedraw(); });
        canvas.on("selection:updated", function () { syncLayers(); refreshProps(); scheduleGridRedraw(); });
        canvas.on("selection:cleared", function () {
          syncLayers();
          setPropEnabled(false);
          document.getElementById("typoLockRow").classList.add("hidden");
          scheduleGridRedraw();
        });

        safeRender();
        console.log("Canvas renderAll initial");
        syncLayers();
        buildPresets();
        pushHistory();
        updateUndoButton();
      }

      document.addEventListener("keydown", function (e) {
        if (e.code === "Space" && !e.target.closest("input, textarea, select")) {
          e.preventDefault();
          spaceDown = true;
        }
        if ((e.key === "Delete" || e.key === "Backspace") && !e.target.closest("input, textarea, select")) {
          var o = canvas && canvas.getActiveObject();
          if (!o) return;
          if (isText(o) && o.isEditing) return;
          if (o.type === "activeSelection") {
            o.getObjects().forEach(function (x) {
              if (!isGuideLayer(x)) canvas.remove(x);
            });
            canvas.discardActiveObject();
          } else if (!isGuideLayer(o)) {
            canvas.remove(o);
            canvas.discardActiveObject();
          }
          safeRender();
          syncLayers();
          scheduleHistoryPush();
        }
        if ((e.metaKey || e.ctrlKey) && (e.key === "z" || e.key === "Z") && !e.shiftKey) {
          var tag = e.target && e.target.tagName;
          if (tag === "INPUT" || tag === "TEXTAREA" || tag === "SELECT") return;
          var ao = canvas && canvas.getActiveObject();
          if (ao && isText(ao) && ao.isEditing) return;
          e.preventDefault();
          doUndo();
        }
      });
      document.addEventListener("keyup", function (e) {
        if (e.code === "Space") spaceDown = false;
      });

      function addImagesFromFiles(files) {
        if (!files || !files.length) return;
        var off = 0;
        Array.from(files).forEach(function (file) {
          var reader = new FileReader();
          reader.onload = function (ev) {
            var url = ev.target.result;
            var imgEl = new Image();
            imgEl.onload = function () {
              loadFabricImage(url).then(function (fi) {
                try {
                  var natW = imgEl.naturalWidth || fi.width;
                  var natH = imgEl.naturalHeight || fi.height;
                  var fit = fitArtboardSize(natW, natH);
                  if (userObjects().filter(function (o) { return o.layerType === "image"; }).length === 0) {
                    resizeArtboard(fit.w, fit.h);
                  }

                  imageSerial++;
                  fi.layerId = genId();
                  fi.layerType = "image";
                  fi.layerLabel = "Image " + imageSerial;

                  var mw = artW * 0.88, mh = artH * 0.88;
                  var sc = Math.min(mw / (fi.width || 1), mh / (fi.height || 1), 1);
                  fi.scale(sc);
                  fi.set({
                    left: (artW - (fi.width * fi.scaleX)) / 2 + off * 18,
                    top: (artH - (fi.height * fi.scaleY)) / 2 + off * 14,
                    cornerColor: "#ffffff",
                    borderColor: "rgba(255,255,255,0.35)",
                    transparentCorners: false
                  });
                  off++;
                  canvas.add(fi);
                  canvas.setActiveObject(fi);
                  syncGuideStackOrder();
                  safeRender();
                  syncLayers();
                  growArtboardToFitAllImages();
                  analyzeImages();
                  scheduleGridRedraw();
                  scrollViewportToShowRulers();
                  console.log("Image loaded", fi.layerLabel);
                } catch (err) { console.warn(err); }
              }).catch(function (err) { console.warn("fromURL", err); });
            };
            imgEl.onerror = function () { console.warn("decode fail"); };
            imgEl.src = url;
          };
          reader.readAsDataURL(file);
        });
        document.getElementById("fileInput").value = "";
      }

      document.getElementById("btnUndo").addEventListener("click", function () {
        doUndo();
      });

      document.getElementById("btnAddImage").addEventListener("click", function () {
        document.getElementById("fileInput").click();
      });
      document.getElementById("btnAddText").addEventListener("click", addBlankTextLayer);
      document.getElementById("fileInput").addEventListener("change", function (e) {
        addImagesFromFiles(e.target.files);
      });

      document.getElementById("btnMerge").addEventListener("click", function () {
        try {
          var act = canvas.getActiveObject();
          if (!act || act.type !== "activeSelection") {
            alert(t("alertNeedMulti"));
            return;
          }
          var parts = act.getObjects().filter(function (o) { return !isGuideLayer(o); });
          if (parts.length < 2) {
            alert(t("alertNeedTwo"));
            return;
          }
          canvas.discardActiveObject();
          parts.forEach(function (p) { try { canvas.remove(p); } catch (e) {} });
          var grp = new Group(parts, { cornerColor: "#fff", borderColor: "rgba(255,255,255,0.4)", subTargetCheck: true });
          grp.layerId = genId();
          grp.layerType = "image";
          grp.layerLabel = "Merged";
          canvas.add(grp);
          canvas.setActiveObject(grp);
          syncGuideStackOrder();
          safeRender();
          syncLayers();
          analyzeImages();
          growArtboardToFitAllImages();
          scheduleGridRedraw();
        } catch (e) {
          console.warn(e);
          alert(t("alertMergeFail"));
        }
      });

      document.getElementById("viewport").addEventListener("wheel", function (e) { e.preventDefault(); }, { passive: false });

      document.getElementById("chkRulers").addEventListener("change", function () {
        rulersVisible = !!this.checked;
        applyRulerChrome();
        renderRulers();
        try {
          document.getElementById("viewport").scrollTop = 0;
        } catch (e) {}
      });
      document.getElementById("chkAlignGrid").addEventListener("change", function () {
        alignGridVisible = !!this.checked;
        if (gridGroup) gridGroup.visible = alignGridVisible;
        if (backGridGroup) backGridGroup.visible = alignGridVisible;
        safeRender();
      });
      document.getElementById("selGuidePalette").addEventListener("change", function () {
        guidePaletteKey = this.value || "mint";
        if (canvas) drawGrid(currentPresetId, getReferenceImage());
      });

      document.getElementById("btnLang").addEventListener("click", function () {
        currentLang = currentLang === "en" ? "zh" : "en";
        applyLanguage();
      });

      fillFontSelect();
      ["pfFont", "pfSizeR", "pfSizeN", "pfR", "pfG", "pfB", "pfHex", "pfOp", "pfLH", "pfCh"].forEach(function (id) {
        document.getElementById(id).addEventListener("input", function () {
          if (id === "pfSizeR") document.getElementById("pfSizeN").value = document.getElementById("pfSizeR").value;
          if (id === "pfSizeN") document.getElementById("pfSizeR").value = document.getElementById("pfSizeN").value;
          if (id === "pfHex") {
            var hx = document.getElementById("pfHex").value;
            if (hx.length >= 7) {
              document.getElementById("pfR").value = parseInt(hx.slice(1, 3), 16);
              document.getElementById("pfG").value = parseInt(hx.slice(3, 5), 16);
              document.getElementById("pfB").value = parseInt(hx.slice(5, 7), 16);
            }
          }
          applyPropsFromUI();
        });
      });
      document.getElementById("pfUnlockPos").addEventListener("change", function () {
        var o = canvas && canvas.getActiveObject();
        if (!o || !isText(o)) return;
        var allow = document.getElementById("pfUnlockPos").checked;
        o.set({ lockMovementX: !allow, lockMovementY: !allow });
        if (allow) o.typographyLocked = false;
        safeRender();
      });

      var modal = document.getElementById("exportModal");
      document.getElementById("btnExport").addEventListener("click", function () {
        modal.classList.remove("hidden");
        modal.classList.add("flex");
        document.getElementById("exportMsg").textContent = "";
        document.getElementById("exportProgress").classList.add("hidden");
      });
      document.getElementById("exportClose").addEventListener("click", function () {
        modal.classList.add("hidden");
        modal.classList.remove("flex");
      });

      function downloadBlob(name, blob) {
        var a = document.createElement("a");
        a.href = URL.createObjectURL(blob);
        a.download = name;
        a.click();
        setTimeout(function () { URL.revokeObjectURL(a.href); }, 1500);
      }

      document.querySelectorAll(".ex-btn").forEach(function (btn) {
        btn.addEventListener("click", function () {
          var fmt = btn.getAttribute("data-fmt");
          document.getElementById("exportProgress").classList.remove("hidden");
          document.getElementById("exportMsg").textContent = t("exportWorking");
          setTimeout(function () {
            try {
              if (fmt === "png" || fmt === "jpg") {
                var gv = gridGroup ? gridGroup.visible : true;
                var gbv = backGridGroup ? backGridGroup.visible : true;
                if (gridGroup) gridGroup.visible = false;
                if (backGridGroup) backGridGroup.visible = false;
                var prevBg = canvas.backgroundColor;
                if (fmt === "png") canvas.backgroundColor = null;
                safeRender();
                var data = canvas.toDataURL({
                  format: fmt === "png" ? "png" : "jpeg",
                  multiplier: 2,
                  quality: fmt === "jpg" ? 0.92 : 1
                });
                if (gridGroup) gridGroup.visible = gv;
                if (backGridGroup) backGridGroup.visible = gbv;
                canvas.backgroundColor = prevBg;
                safeRender();
                var raw = data.split(",")[1];
                var bin = atob(raw);
                var u8 = new Uint8Array(bin.length);
                for (var i = 0; i < bin.length; i++) u8[i] = bin.charCodeAt(i);
                downloadBlob("export." + (fmt === "png" ? "png" : "jpg"), new Blob([u8], { type: fmt === "png" ? "image/png" : "image/jpeg" }));
                document.getElementById("exportMsg").textContent = t("exportDownloaded");
                showToast(t("toastExportOk"));
              } else {
                var payload = {
                  format: fmt === "ai" ? "smart-grid-ai-json" : "smart-grid-psd-json",
                  version: 1,
                  artboard: { width: artW, height: artH },
                  document: canvas.toJSON(["layerId", "layerType", "layerLabel"])
                };
                downloadBlob("export." + (fmt === "ai" ? "ai.json" : "psd.json"), new Blob([JSON.stringify(payload, null, 2)], { type: "application/json" }));
                document.getElementById("exportMsg").textContent = currentLang === "en" ? "JSON downloaded." : "JSON 已下載。";
                showToast(t("toastExportOk"));
              }
            } catch (e) {
              document.getElementById("exportMsg").textContent = (currentLang === "en" ? "Failed: " : "失敗：") + (e.message || e);
              showToast(t("toastExportFail"));
            }
            document.getElementById("exportProgress").classList.add("hidden");
          }, 400);
        });
      });

      try {
        initCanvas();
        applyLanguage();
      } catch (e) {
        console.error(e);
        var errLabel = currentLang === "en" ? "Error: " : "錯誤：";
        document.body.insertAdjacentHTML("beforeend", '<div class="fixed bottom-4 left-4 right-4 z-[300] bg-red-900/90 text-sm p-3 rounded-lg">' + errLabel + (e.message || e) + "</div>");
      }
    });
  </script>
</body>
</html>
