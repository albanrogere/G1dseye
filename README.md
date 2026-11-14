<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GOD'S EYE — Aerial & Maritime Intelligence</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
<script>window.CESIUM_BASE_URL='https://unpkg.com/cesium@1.106.0/Build/Cesium/';</script>
<script src="https://unpkg.com/cesium@1.106.0/Build/Cesium/Cesium.js"></script>
<link href="https://unpkg.com/cesium@1.106.0/Build/Cesium/Widgets/widgets.css" rel="stylesheet">
<style>
:root{
  --bg0:#060606;--bg1:#0d0d0d;--bg2:#141414;--bg3:#1b1b1b;--bg4:#222222;--bg5:#2a2a2a;
  --b1:#1e1e1e;--b2:#282828;--b3:#353535;
  --t1:#eeeeee;--t2:#888888;--t3:#4e4e4e;--t4:#2e2e2e;
  --acc:#ffffff;--adim:rgba(255,255,255,0.04);
  --red:#c44040;--rdim:rgba(196,64,64,0.09);
  --amb:#b08828;--grn:#3a9a3a;--pur:#7248e4;--blu:#4fa3e0;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;overflow:hidden;-webkit-font-smoothing:antialiased}
body{background:var(--bg0);color:var(--t1);font-family:'Inter',sans-serif;font-size:12px;line-height:1.45}
::-webkit-scrollbar{width:3px;height:3px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--bg5);border-radius:2px}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.2}}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes fadein{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:translateY(0)}}
@keyframes pulseRing{0%{transform:scale(1);opacity:.7}100%{transform:scale(2.2);opacity:0}}

/* ===== CESIUM OVERRIDES ===== */
.cesium-widget-credits{display:none!important}
.cesium-viewer-toolbar,.cesium-viewer-animationContainer,.cesium-viewer-timelineContainer,
.cesium-viewer-bottom,.cesium-viewer-fullscreenContainer{display:none!important}

/* ===== APP SHELL ===== */
#app{display:flex;flex-direction:column;height:100vh;width:100vw}

/* ===== MENUBAR ===== */
#menubar{
  height:40px;flex-shrink:0;
  background:var(--bg1);border-bottom:1px solid var(--b1);
  display:flex;align-items:center;z-index:200;user-select:none;
}
.mb-brand{display:flex;align-items:center;gap:9px;padding:0 16px;border-right:1px solid var(--b1);height:100%}
.mb-t{font-size:12px;font-weight:700;letter-spacing:.1em;color:var(--acc);font-family:'JetBrains Mono',monospace}
.mb-s{font-size:8px;color:var(--t3);letter-spacing:.14em;text-transform:uppercase;margin-top:1px}
.mb-tabs{display:flex;align-items:stretch;height:100%;margin-left:2px}
.mb-tab{display:flex;align-items:center;padding:0 14px;font-size:11px;font-weight:500;color:var(--t3);cursor:pointer;border-bottom:2px solid transparent;transition:all .1s;border:none;background:transparent;font-family:'Inter',sans-serif;text-decoration:none}
.mb-tab:hover{color:var(--t1);background:var(--adim)}
.mb-tab.on{color:var(--acc);border-bottom-color:var(--acc)}
.mb-tab.globe-tab{color:var(--blu)}
.mb-tab.globe-tab.on{color:var(--blu);border-bottom-color:var(--blu)}
.mb-right{margin-left:auto;display:flex;align-items:center;gap:7px;padding:0 12px}
.mb-badge{display:flex;align-items:center;gap:5px;padding:3px 8px;background:var(--bg3);border:1px solid var(--b2);border-radius:3px;font-size:10px;color:var(--t2);font-family:'JetBrains Mono',monospace}
.mb-led{width:6px;height:6px;border-radius:50%;background:var(--grn);flex-shrink:0}
.mb-btn{height:26px;padding:0 10px;background:var(--bg3);border:1px solid var(--b2);color:var(--t1);font-size:11px;font-weight:500;cursor:pointer;border-radius:3px;transition:all .1s;font-family:'Inter',sans-serif}
.mb-btn:hover{background:var(--bg4);border-color:var(--b3)}

/* ===== CMDBAR (2D only) ===== */
#cmdbar{
  height:32px;flex-shrink:0;
  background:var(--bg1);border-bottom:1px solid var(--b1);
  display:flex;align-items:center;padding:0 8px;gap:0;
}
.cg{display:flex;align-items:center;gap:2px;padding:0 7px;border-right:1px solid var(--b1);height:100%}
.cg-lbl{font-size:9px;letter-spacing:.12em;text-transform:uppercase;color:var(--t4);margin-right:3px;white-space:nowrap}
.rb{height:21px;padding:0 8px;background:transparent;border:1px solid transparent;color:var(--t3);font-size:11px;font-weight:500;cursor:pointer;border-radius:2px;transition:all .1s;font-family:'Inter',sans-serif}
.rb:hover{background:var(--bg3);color:var(--t1);border-color:var(--b2)}
.rb.on{background:var(--bg4);color:var(--acc);border-color:var(--b3);font-weight:600}
.cs{display:flex;align-items:center;gap:4px;padding:0 7px;height:100%;border-right:1px solid var(--b1);font-size:11px}
.cs-lbl{color:var(--t4);font-size:9px}
.cs-val{color:var(--t1);font-weight:600;font-family:'JetBrains Mono',monospace}
.cs-val.r{color:var(--red)}.cs-val.a{color:var(--amb)}.cs-val.g{color:var(--grn)}
.cb-time{margin-left:auto;font-size:10px;color:var(--t4);font-family:'JetBrains Mono',monospace;padding:0 8px}

/* ===== 2D BODY ===== */
#view-2d{flex:1;display:flex;overflow:hidden;min-height:0}
#view-3d{display:none;flex:1;position:relative;overflow:hidden}

/* ===== SIDEBAR TABS ===== */
#sb{width:245px;flex-shrink:0;background:var(--bg1);border-right:1px solid var(--b1);display:flex;flex-direction:column;overflow:hidden;}
#sb-mode{display:flex;flex-shrink:0;border-bottom:1px solid var(--b1);background:var(--bg2);flex-wrap:wrap}
.sb-mode-btn{flex:1;min-width:33%;height:30px;background:transparent;border:none;border-bottom:2px solid transparent;color:var(--t3);font-size:10px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;cursor:pointer;transition:all .1s;font-family:'Inter',sans-serif;display:flex;align-items:center;justify-content:center;gap:4px}
.sb-mode-btn:hover{color:var(--t1);background:var(--adim)}
.sb-mode-btn.on.aer{color:var(--acc);border-bottom-color:var(--acc)}
.sb-mode-btn.on.mar{color:var(--sh-cnt);border-bottom-color:var(--sh-cnt)}
.sb-mode-btn.on.all{color:#c8a030;border-bottom-color:#c8a030}
/* Aerial pane */
#sb-aerial{display:flex;flex-direction:column;flex:1;overflow:hidden;min-height:0}
/* Maritime pane */
#sb-maritime{display:none;flex-direction:column;flex:1;overflow:hidden;min-height:0}
/* Ship list */
.sh-item{display:grid;grid-template-columns:4px 1fr 54px;border-bottom:1px solid var(--b1);cursor:pointer;transition:background .07s;min-height:40px}
.sh-item:hover{background:var(--bg2)}
.sh-item.sel{background:rgba(74,144,217,.06)}
.sh-bar{width:4px}
.sh-b{padding:5px 9px;min-width:0}
.sh-nm{font-size:11px;font-weight:600;font-family:'JetBrains Mono',monospace;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;letter-spacing:.01em}
.sh-meta{font-size:9px;color:var(--t3);margin-top:2px;display:flex;gap:5px;align-items:center}
.sh-ano{color:var(--red);font-weight:700;font-size:9px}
.sh-r{padding:4px 7px 4px 0;text-align:right;display:flex;flex-direction:column;justify-content:center;gap:2px}
.sh-spd{font-size:10px;font-family:'JetBrains Mono',monospace;color:var(--t2)}
.sh-len{font-size:9px;color:var(--t4)}
/* Port list */
.port-item{display:grid;grid-template-columns:4px 1fr 44px;border-bottom:1px solid var(--b1);cursor:pointer;transition:background .07s;min-height:36px}
.port-item:hover{background:var(--bg2)}
.port-item.sel{background:rgba(212,160,32,.06)}
.port-bar{width:4px}
.port-b{padding:5px 9px;min-width:0}
.port-nm-s{font-size:11px;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.port-meta{font-size:9px;color:var(--t3);margin-top:2px}
.port-r{padding:4px 6px 4px 0;text-align:right;display:flex;flex-direction:column;justify-content:center;gap:2px}
.port-cap-s{font-size:9px;color:var(--t2);font-family:'JetBrains Mono',monospace}
/* Section separator in maritime pane */
.sb-section-hd{height:26px;flex-shrink:0;background:var(--bg2);border-bottom:1px solid var(--b1);display:flex;align-items:center;justify-content:space-between;padding:0 10px}
.sb-section-ht{font-size:8px;font-weight:700;letter-spacing:.14em;text-transform:uppercase}
.sb-section-ct{font-size:10px;color:var(--t4);font-family:'JetBrains Mono',monospace}
/* Maritime search/filters */
.mar-srch{padding:5px 7px;border-bottom:1px solid var(--b1);flex-shrink:0}
#mar-input{width:100%;height:24px;padding:0 8px;background:var(--bg2);border:1px solid var(--b2);color:var(--t1);border-radius:2px;font-size:11px;font-family:'Inter',sans-serif;outline:none}
#mar-input::placeholder{color:var(--t4)}
.mar-chips{display:flex;gap:2px;padding:4px 7px;border-bottom:1px solid var(--b1);flex-shrink:0;flex-wrap:wrap}
/* Anomaly indicator dot */
.ano-dot{width:5px;height:5px;border-radius:50%;background:var(--red);flex-shrink:0;animation:blink 1s infinite}
.sb-hd{height:32px;flex-shrink:0;background:var(--bg2);border-bottom:1px solid var(--b1);display:flex;align-items:center;justify-content:space-between;padding:0 11px}
.sb-ht{font-size:9px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--t2)}
.sb-ct{font-size:10px;color:var(--t4);font-family:'JetBrains Mono',monospace}
.sb-srch{padding:6px 8px;border-bottom:1px solid var(--b1);flex-shrink:0}
#s-input{width:100%;height:26px;padding:0 9px;background:var(--bg2);border:1px solid var(--b2);color:var(--t1);border-radius:2px;font-size:11px;font-family:'Inter',sans-serif;outline:none;transition:border-color .15s}
#s-input:focus{border-color:var(--b3)}
#s-input::placeholder{color:var(--t4)}
.sb-chips{display:flex;gap:2px;padding:5px 7px;border-bottom:1px solid var(--b1);flex-shrink:0}
.chip{height:20px;padding:0 7px;background:transparent;border:1px solid var(--b2);color:var(--t4);font-size:10px;cursor:pointer;border-radius:2px;transition:all .1s;font-family:'Inter',sans-serif}
.chip:hover{background:var(--bg3);color:var(--t1)}
.chip.on{background:var(--bg4);color:var(--acc);border-color:var(--b3)}
.sb-th{display:grid;grid-template-columns:3px 1fr 50px;background:var(--bg2);border-bottom:1px solid var(--b1);flex-shrink:0}
.sb-th-c{padding:4px 9px;font-size:8px;font-weight:700;letter-spacing:.12em;text-transform:uppercase;color:var(--t4)}
#fl-list{flex:1;overflow-y:auto}
.fl{display:grid;grid-template-columns:3px 1fr 50px;border-bottom:1px solid var(--b1);cursor:pointer;transition:background .07s;min-height:37px}
.fl:hover{background:var(--bg2)}
.fl.sel{background:rgba(255,255,255,.04)}
.fl-bar{width:3px}
.fl-b{padding:6px 9px;min-width:0}
.fl-cs{font-size:11px;font-weight:600;font-family:'JetBrains Mono',monospace;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;letter-spacing:.02em}
.fl-m{font-size:9px;color:var(--t3);margin-top:2px;display:flex;gap:4px}
.fl-r{padding:5px 7px 5px 0;text-align:right}
.fl-alt{font-size:10px;font-family:'JetBrains Mono',monospace;color:var(--t2)}
.fl-ago{font-size:9px;color:var(--t4)}
#err-box{padding:8px 10px;background:var(--rdim);border-bottom:1px solid rgba(196,64,64,.16);color:#a06060;font-size:10px;line-height:1.6;display:none}
#sb-ft{padding:6px 10px;border-top:1px solid var(--b1);font-size:9px;color:var(--t4);line-height:1.8;flex-shrink:0}

/* ===== MAP AREA ===== */
#mw{flex:1;position:relative;overflow:hidden;background:var(--bg0)}
#mc{display:block;cursor:crosshair}
#minfo{position:absolute;top:8px;left:8px;font-size:8px;color:rgba(255,255,255,.11);font-family:'JetBrains Mono',monospace;line-height:1.9;pointer-events:none}
#htip{position:absolute;bottom:40px;left:8px;background:var(--bg2);border:1px solid var(--b2);border-radius:3px;padding:7px 11px;font-size:11px;color:var(--t1);pointer-events:none;display:none;box-shadow:0 4px 18px rgba(0,0,0,.5);line-height:1.7}
#leg{position:absolute;bottom:8px;left:8px;background:var(--bg2);border:1px solid var(--b2);border-radius:3px;padding:11px 13px;font-size:10px;color:var(--t2);min-width:162px;box-shadow:0 4px 18px rgba(0,0,0,.4);z-index:20}
.leg-hd{font-size:9px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--t1);margin-bottom:8px;display:flex;justify-content:space-between;align-items:center}
.leg-x{cursor:pointer;color:var(--t3);font-size:12px;padding:0 2px;line-height:1}
.leg-x:hover{color:var(--t1)}
.leg-sec{font-size:8px;letter-spacing:.14em;text-transform:uppercase;color:var(--t4);margin:6px 0 4px}
.leg-row{display:flex;align-items:center;gap:7px;margin-bottom:3px;font-size:10px;color:var(--t2)}
.leg-dot{width:8px;height:8px;border-radius:2px;flex-shrink:0}
.leg-line{width:22px;height:2px;flex-shrink:0;border-radius:1px}
#leg-show{position:absolute;bottom:8px;left:8px;display:none;background:var(--bg2);border:1px solid var(--b2);border-radius:3px;padding:5px 10px;font-size:10px;color:var(--t1);cursor:pointer;z-index:20;font-family:'Inter',sans-serif}
#loader{position:absolute;inset:0;background:rgba(6,6,6,.92);display:flex;align-items:center;justify-content:center;z-index:90;flex-direction:column;gap:13px}
.ld-sp{width:28px;height:28px;border:2px solid var(--bg5);border-top-color:var(--t2);border-radius:50%;animation:spin .8s linear infinite}
.ld-tx{font-size:10px;letter-spacing:.2em;text-transform:uppercase;color:var(--t3);font-weight:500}

/* ===== GLOBE LOADER ===== */
#globe-loader{position:absolute;inset:0;background:#060606;z-index:500;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px}
.gl-sp{width:34px;height:34px;border:3px solid var(--bg5);border-top-color:var(--t2);border-radius:50%;animation:spin 1s linear infinite}
.gl-title{font-size:13px;font-weight:700;letter-spacing:.2em;text-transform:uppercase;color:var(--acc);font-family:'JetBrains Mono',monospace}
.gl-sub{font-size:9px;color:var(--t4);letter-spacing:.16em;text-transform:uppercase}
#cesiumContainer{width:100%;height:100%;background:#060606}

/* ===== GLOBE CONTROLS PANEL ===== */
#gctrl{
  position:absolute;left:0;top:0;bottom:0;width:260px;
  background:rgba(8,8,8,.94);backdrop-filter:blur(10px);
  border-right:1px solid var(--b1);
  display:flex;flex-direction:column;overflow:hidden;
  transform:translateX(-100%);
  transition:transform .22s cubic-bezier(.4,0,.2,1);
  z-index:100;
}
#gctrl.open{transform:translateX(0)}
#gctrl-toggle{
  position:absolute;left:0;top:50%;transform:translateY(-50%);
  width:20px;height:44px;
  background:rgba(8,8,8,.9);border:1px solid var(--b2);border-left:none;
  color:var(--t3);font-size:10px;cursor:pointer;
  border-radius:0 3px 3px 0;display:flex;align-items:center;justify-content:center;
  transition:left .22s cubic-bezier(.4,0,.2,1);z-index:101;
}
#gctrl-toggle.open{left:260px}
#gctrl-toggle:hover{color:var(--t1)}
.gc-scroll{flex:1;overflow-y:auto;padding:0}
.gc-search{padding:8px 10px 6px;border-bottom:1px solid var(--b1);flex-shrink:0}
.gc-search-row{display:flex;gap:4px}
#gc-search-input{flex:1;height:27px;padding:0 9px;background:var(--bg2);border:1px solid var(--b2);color:var(--t1);border-radius:2px;font-size:11px;font-family:'Inter',sans-serif;outline:none;transition:border-color .12s}
#gc-search-input:focus{border-color:var(--b3)}
#gc-search-input::placeholder{color:var(--t4)}
#gc-search-btn{width:27px;height:27px;background:var(--bg3);border:1px solid var(--b2);color:var(--t1);cursor:pointer;border-radius:2px;font-size:11px;flex-shrink:0}
#gc-search-results{max-height:150px;overflow-y:auto;border-bottom:1px solid var(--b1);display:none}
.gsr-item{padding:7px 10px;cursor:pointer;border-bottom:1px solid var(--b1);transition:background .07s}
.gsr-item:hover{background:var(--bg2)}
.gsr-name{font-size:11px;color:var(--t1)}
.gsr-sub{font-size:9px;color:var(--t3);margin-top:1px}
.gc-section{padding:8px 10px;border-bottom:1px solid var(--b1)}
.gc-sec-title{font-size:8px;font-weight:700;letter-spacing:.16em;text-transform:uppercase;color:var(--t3);margin-bottom:7px}
.img-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:3px}
.img-btn{padding:5px 0;background:var(--bg3);border:1px solid var(--b2);color:var(--t3);font-size:9px;font-weight:600;cursor:pointer;border-radius:2px;text-align:center;transition:all .1s;font-family:'Inter',sans-serif}
.img-btn:hover{background:var(--bg4);color:var(--t1)}
.img-btn.on{background:var(--bg4);color:var(--acc);border-color:var(--b3)}
.tog-row{display:flex;align-items:center;justify-content:space-between;padding:4px 0;border-bottom:1px solid rgba(255,255,255,.03)}
.tog-lbl{font-size:11px;color:var(--t2);display:flex;align-items:center;gap:7px}
.tog-dot{width:6px;height:6px;border-radius:50%;flex-shrink:0}
.tog-sw{position:relative;width:32px;height:17px;cursor:pointer;flex-shrink:0}
.tog-sw input{opacity:0;width:0;height:0}
.tog-slider{position:absolute;inset:0;background:var(--bg4);border:1px solid var(--b3);border-radius:9px;transition:background .15s}
.tog-slider:before{content:'';position:absolute;width:11px;height:11px;left:2px;top:2px;background:var(--t3);border-radius:50%;transition:all .15s}
.tog-sw input:checked + .tog-slider{background:var(--bg5)}
.tog-sw input:checked + .tog-slider:before{transform:translateX(15px);background:var(--acc)}
.fchip-row{display:flex;gap:3px;flex-wrap:wrap;margin-bottom:7px}
.fchip{height:20px;padding:0 7px;background:transparent;border:1px solid var(--b2);color:var(--t4);font-size:10px;cursor:pointer;border-radius:2px;transition:all .1s;font-family:'Inter',sans-serif}
.fchip:hover{background:var(--bg3);color:var(--t1)}
.fchip.on{background:var(--bg4);color:var(--acc);border-color:var(--b3)}
.range-row{margin:5px 0}
.range-lbl{display:flex;justify-content:space-between;font-size:9px;color:var(--t3);margin-bottom:3px}
.range-lbl span{font-family:'JetBrains Mono',monospace;color:var(--t2)}
input[type=range]{width:100%;height:3px;-webkit-appearance:none;background:var(--bg4);border-radius:2px;outline:none}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:12px;height:12px;border-radius:50%;background:var(--acc);cursor:pointer;border:1px solid var(--b3)}
.gc-stats{display:grid;grid-template-columns:1fr 1fr;gap:4px}
.gc-stat{background:var(--bg2);border:1px solid var(--b1);border-radius:3px;padding:6px 8px;text-align:center}
.gc-stat-v{font-size:14px;font-weight:700;font-family:'JetBrains Mono',monospace;color:var(--acc)}
.gc-stat-v.r{color:var(--red)}.gc-stat-v.a{color:var(--amb)}.gc-stat-v.b{color:var(--blu)}
.gc-stat-l{font-size:8px;text-transform:uppercase;letter-spacing:.1em;color:var(--t4);margin-top:1px}
.tool-grid{display:grid;grid-template-columns:1fr 1fr;gap:4px}
.tool-btn{padding:6px 4px;background:var(--bg3);border:1px solid var(--b2);color:var(--t2);font-size:10px;cursor:pointer;border-radius:2px;text-align:center;transition:all .1s;font-family:'Inter',sans-serif}
.tool-btn:hover{background:var(--bg4);color:var(--t1);border-color:var(--b3)}
.tool-btn.active{background:var(--bg5);color:var(--acc);border-color:var(--acc)}
.city-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:3px}
.city-btn{padding:5px 0;background:var(--bg3);border:1px solid var(--b2);color:var(--t3);font-size:9px;cursor:pointer;border-radius:2px;text-align:center;transition:all .1s;font-family:'Inter',sans-serif}
.city-btn:hover{background:var(--bg4);color:var(--t1)}
.token-input{width:100%;height:26px;padding:0 8px;background:var(--bg3);border:1px solid var(--b2);color:var(--t1);font-size:10px;border-radius:2px;font-family:'JetBrains Mono',monospace;outline:none;margin-top:4px}
.apply-btn{width:100%;height:24px;margin-top:4px;background:var(--bg4);border:1px solid var(--b2);color:var(--t1);font-size:10px;cursor:pointer;border-radius:2px;transition:all .1s;font-family:'Inter',sans-serif}
.apply-btn:hover{background:var(--bg5)}

/* ===== GLOBE FLOATING NAV ===== */
#globe-nav{position:absolute;right:12px;bottom:44px;z-index:100;display:flex;flex-direction:column;gap:3px;transition:right .22s}
#globe-nav.panel-open{right:352px}
.gnav-btn{width:34px;height:34px;background:rgba(8,8,8,.9);border:1px solid var(--b2);color:var(--t2);cursor:pointer;border-radius:3px;font-size:16px;display:flex;align-items:center;justify-content:center;transition:all .1s;backdrop-filter:blur(6px)}
.gnav-btn:hover{background:rgba(20,20,20,.95);color:var(--t1);border-color:var(--b3)}
#compass-wrap{width:42px;height:42px;position:relative;cursor:pointer;margin:4px auto;display:flex;align-items:center;justify-content:center}
#compass-ring{width:42px;height:42px;border-radius:50%;border:1px solid var(--b2);display:flex;align-items:center;justify-content:center;background:rgba(8,8,8,.8);transition:transform .3s}
.c-n{font-size:9px;font-weight:700;color:var(--red);font-family:'JetBrains Mono',monospace}

/* ===== GLOBE HUD ===== */
#globe-hud{position:absolute;bottom:0;left:0;right:0;height:28px;background:rgba(6,6,6,.88);backdrop-filter:blur(6px);border-top:1px solid var(--b1);display:flex;align-items:center;padding:0 12px;gap:0;z-index:100}
.hud-item{padding:0 9px;border-right:1px solid var(--b1);font-size:9px;font-family:'JetBrains Mono',monospace;color:var(--t4);height:100%;display:flex;align-items:center;white-space:nowrap}
.hud-item span{color:var(--t2);margin-left:4px}
#hud-scale-wrap{margin-left:auto;display:flex;align-items:center;gap:6px;padding:0 9px}
#hud-scale-line{height:5px;border:1px solid var(--t4);border-top:none;width:50px}
#hud-scale-lbl{font-size:9px;font-family:'JetBrains Mono',monospace;color:var(--t4)}

/* ===== GLOBE NOTIF ===== */
#globe-notif{position:absolute;top:12px;left:50%;transform:translateX(-50%);background:rgba(8,8,8,.93);border:1px solid var(--b2);border-radius:3px;padding:7px 16px;font-size:11px;color:var(--t1);z-index:300;display:none;pointer-events:none;backdrop-filter:blur(6px);white-space:nowrap;animation:fadein .2s ease}

/* ===== MEASURE HINT ===== */
#measure-hint{position:absolute;bottom:36px;left:50%;transform:translateX(-50%);background:rgba(8,8,8,.92);border:1px solid var(--b2);border-radius:3px;padding:7px 14px;font-size:11px;color:var(--t1);z-index:200;display:none;pointer-events:none;font-family:'JetBrains Mono',monospace;letter-spacing:.04em}

/* ===== RIGHT DETAIL PANEL ===== */
#det{
  position:fixed;right:0;
  top:72px;
  bottom:0;width:340px;
  background:var(--bg1);
  border-left:1px solid var(--b2);
  display:flex;flex-direction:column;overflow:hidden;
  transform:translateX(100%);
  transition:transform .22s cubic-bezier(.4,0,.2,1);
  z-index:150;
  box-shadow:-6px 0 28px rgba(0,0,0,.55);
}
#det.open{transform:translateX(0)}
#det.globe-mode{top:40px}

/* Tab trigger handle */
#det-tab{
  position:fixed;right:0;top:50%;
  transform:translateY(-50%);
  width:24px;padding:18px 0;
  background:var(--bg2);border:1px solid var(--b2);
  border-right:none;border-radius:4px 0 0 4px;
  cursor:pointer;z-index:149;
  display:flex;flex-direction:column;align-items:center;justify-content:center;gap:3px;
  transition:background .1s,opacity .2s;
}
#det-tab:hover{background:var(--bg3)}
#det-tab.hidden{opacity:0;pointer-events:none}
.det-tab-txt{writing-mode:vertical-rl;text-orientation:mixed;transform:rotate(180deg);font-size:8px;font-weight:700;letter-spacing:.18em;text-transform:uppercase;color:var(--t3);font-family:'JetBrains Mono',monospace}
.det-tab-dot{width:5px;height:5px;border-radius:50%;background:var(--b3)}
#det-hd{flex-shrink:0;background:var(--bg2);border-bottom:1px solid var(--b1);}
#det-empty{padding:18px 14px 14px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:6px;text-align:center;}
#det-empty-icon{width:38px;height:38px;border:1px solid var(--b2);border-radius:50%;display:flex;align-items:center;justify-content:center;margin-bottom:4px;}
#det-empty-title{font-size:11px;font-weight:600;color:var(--t2);letter-spacing:.04em}
#det-empty-sub{font-size:10px;color:var(--t4);line-height:1.5}
#det-flight{display:none;padding:12px 14px 10px}
#det-cs{font-size:15px;font-weight:700;font-family:'JetBrains Mono',monospace;letter-spacing:.04em}
#det-meta{display:flex;align-items:center;gap:6px;margin-top:3px}
.d-badge{display:inline-flex;align-items:center;height:17px;padding:0 6px;border-radius:2px;font-size:8px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;border:1px solid transparent}
#det-cy{font-size:9px;color:var(--t3)}
#det-cc{font-size:9px;font-family:'JetBrains Mono',monospace;color:var(--t3);font-weight:700}

/* Tab nav */
#tab-nav{display:flex;border-bottom:1px solid var(--b1);background:var(--bg2);flex-shrink:0;}
.tab-btn{flex:1;height:32px;background:transparent;border:none;border-bottom:2px solid transparent;color:var(--t3);font-size:10px;font-weight:600;letter-spacing:.08em;text-transform:uppercase;cursor:pointer;transition:all .1s;font-family:'Inter',sans-serif;}
.tab-btn:hover{color:var(--t1);background:var(--adim)}
.tab-btn.on{color:var(--acc);border-bottom-color:var(--acc);background:var(--adim)}
#det-body{flex:1;overflow:hidden;position:relative}
.tab-pane{position:absolute;inset:0;overflow-y:auto;display:none;animation:fadein .15s ease}
.tab-pane.on{display:block}

/* Route pane */
.pane-pad{padding:12px 13px}
.route{display:flex;align-items:stretch;margin-bottom:10px}
.ap{text-align:center;flex:1;min-width:0}
.ap-code{font-size:21px;font-weight:800;font-family:'JetBrains Mono',monospace;color:var(--acc);line-height:1;letter-spacing:.04em}
.ap-city{font-size:8px;color:var(--t3);margin-top:2px;text-transform:uppercase;letter-spacing:.07em;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.ap-time{font-size:12px;font-weight:700;font-family:'JetBrains Mono',monospace;color:var(--t1);margin-top:4px}
.r-mid{flex:2;padding:0 7px;display:flex;flex-direction:column;justify-content:center}
.r-fn{font-size:10px;font-weight:600;text-align:center;color:var(--t2);margin-bottom:5px;font-family:'JetBrains Mono',monospace;letter-spacing:.04em}
.pb-wrap{height:6px;background:var(--bg4);border-radius:3px;overflow:visible;border:1px solid var(--b2);position:relative}
.pb-fill{height:100%;background:var(--acc);border-radius:3px;transition:width 1s;min-width:6px;position:relative}
.pb-arrow{position:absolute;right:-8px;top:-8px;font-size:13px;font-family:'JetBrains Mono',monospace;color:var(--acc);font-weight:900;line-height:1}
.pb-pct{text-align:center;font-size:9px;font-family:'JetBrains Mono',monospace;color:var(--t3);margin-top:4px}
.tg{display:grid;grid-template-columns:repeat(4,1fr);gap:4px;margin-top:9px}
.tc{background:var(--bg2);border:1px solid var(--b1);border-radius:3px;padding:7px 4px;text-align:center}
.tc-l{font-size:7px;text-transform:uppercase;letter-spacing:.12em;color:var(--t3);margin-bottom:3px}
.tc-v{font-size:12px;font-weight:700;font-family:'JetBrains Mono',monospace;color:var(--acc)}
.tc-v.hl{color:var(--amb)}
.pane-div{height:1px;background:var(--b1);margin:10px 0}
.pane-lbl{font-size:8px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--t3);margin-bottom:7px}

/* Live pane */
#zc{width:100%;border-radius:3px;border:1px solid var(--b1);display:block;background:var(--bg0)}
.z-hint{font-size:8px;color:var(--t4);text-align:center;margin-top:3px;margin-bottom:10px}
.gg{display:grid;grid-template-columns:1fr 1fr;gap:6px}
.gc{background:var(--bg2);border:1px solid var(--b1);border-radius:3px;padding:8px 9px 7px}
.gc-l{font-size:8px;text-transform:uppercase;letter-spacing:.12em;color:var(--t3);margin-bottom:4px}
.gc-r{display:flex;align-items:baseline;gap:3px}
.gc-v{font-size:17px;font-weight:700;font-family:'JetBrains Mono',monospace;color:var(--acc);line-height:1}
.gc-u{font-size:9px;color:var(--t3)}
.gc-tr{height:2px;background:var(--bg4);border-radius:1px;margin-top:6px;overflow:hidden}
.gc-b{height:100%;background:var(--t1);border-radius:1px;transition:width .6s}
.gc-b.r{background:var(--red)}.gc-b.g{background:var(--grn)}.gc-b.a{background:var(--amb)}

/* AI pane */
#ai-trig{display:flex;align-items:center;gap:8px;padding:11px 13px;margin:0;background:var(--bg2);border:1px solid var(--b2);border-radius:3px;cursor:pointer;transition:background .1s;margin-bottom:10px;}
#ai-trig:hover{background:var(--bg3)}
.ai-led{width:6px;height:6px;border-radius:50%;background:var(--pur);animation:blink 2s infinite;flex-shrink:0}
.ai-hl{font-weight:600;font-size:11px;color:var(--t1);letter-spacing:.03em}
.ai-arr{margin-left:auto;font-size:10px;color:var(--t3);font-family:'JetBrains Mono',monospace}
#ai-bd{display:none}
#ai-ld{display:none;align-items:center;gap:7px;font-size:10px;color:var(--t3);margin-bottom:8px}
.ai-sp{width:12px;height:12px;border:1.5px solid var(--bg5);border-top-color:var(--pur);border-radius:50%;animation:spin .7s linear infinite;flex-shrink:0}
#ai-sum{font-size:11px;color:var(--t2);line-height:1.7;white-space:pre-wrap;margin-bottom:8px}
.ai-tbl{width:100%;border-collapse:collapse;margin-bottom:8px}
.ai-tbl tr{border-bottom:1px solid var(--b1)}
.ai-tbl td{padding:5px 0;font-size:10px}
.ai-tbl td:first-child{color:var(--t3);text-transform:uppercase;letter-spacing:.1em;width:42%;font-size:9px}
.ai-tbl td:last-child{color:var(--t1);font-weight:500;font-family:'JetBrains Mono',monospace}
#ph-wr{margin-top:2px;display:none}
.ph-l{font-size:8px;text-transform:uppercase;letter-spacing:.14em;color:var(--t3);margin-bottom:5px}
#ph-gr{display:grid;grid-template-columns:1fr 1fr;gap:4px}
.ph-img{width:100%;aspect-ratio:16/9;object-fit:cover;border-radius:3px;border:1px solid var(--b1);cursor:pointer;transition:border-color .12s}
.ph-img:hover{border-color:var(--b3)}
.raw-tbl{width:100%;border-collapse:collapse}
.raw-tbl tr{border-bottom:1px solid var(--b1)}
.raw-tbl td{padding:5px 0;font-size:10px}
.raw-tbl td:first-child{color:var(--t3);text-transform:uppercase;letter-spacing:.1em;width:44%;font-size:9px}
.raw-tbl td:last-child{color:var(--t1);font-family:'JetBrains Mono',monospace}
.ext-link{display:block;text-align:center;padding:7px;background:var(--bg3);border:1px solid var(--b2);color:var(--t2);text-decoration:none;border-radius:3px;font-size:10px;letter-spacing:.1em;text-transform:uppercase;font-weight:500;transition:all .13s;margin-top:10px}
.ext-link:hover{background:var(--bg4);color:var(--t1);border-color:var(--b3)}

/* Airport panel */
#det-ap-hd{display:none;padding:12px 14px 0}
.ap-iata{font-size:22px;font-weight:800;font-family:'JetBrains Mono',monospace;color:#4fa3e0;letter-spacing:.05em;line-height:1}
.ap-fullname{font-size:11px;font-weight:600;color:var(--t1);margin-top:3px}
.ap-loc{font-size:9px;color:var(--t3);margin-top:2px}
.ap-stats-row{display:flex;gap:6px;margin-top:8px;padding-bottom:10px}
.ap-stat{flex:1;background:var(--bg3);border:1px solid var(--b2);border-radius:3px;padding:5px 7px;text-align:center}
.ap-stat-v{font-size:12px;font-weight:700;font-family:'JetBrains Mono',monospace;color:#4fa3e0}
.ap-stat-l{font-size:7px;text-transform:uppercase;letter-spacing:.1em;color:var(--t4);margin-top:1px}
#ap-tab-nav{display:none;border-bottom:1px solid var(--b1);background:var(--bg2)}
.ap-tab-btn{flex:1;height:30px;background:transparent;border:none;border-bottom:2px solid transparent;color:var(--t3);font-size:10px;font-weight:600;letter-spacing:.07em;text-transform:uppercase;cursor:pointer;transition:all .1s;font-family:'Inter',sans-serif}
.ap-tab-btn:hover{color:var(--t1);background:var(--adim)}
.ap-tab-btn.on{color:#4fa3e0;border-bottom-color:#4fa3e0;background:var(--adim)}
#ap-body{flex:1;overflow:hidden;position:relative;display:none}
.ap-pane{position:absolute;inset:0;overflow-y:auto;display:none;animation:fadein .15s ease}
.ap-pane.on{display:block}
.ap-section{padding:10px 13px;border-bottom:1px solid var(--b1)}
.ap-sec-title{font-size:8px;font-weight:700;letter-spacing:.16em;text-transform:uppercase;color:var(--t3);margin-bottom:7px}
.ap-kv{display:grid;grid-template-columns:42% 1fr;gap:0}
.ap-kv-row{display:contents}
.ap-kv-row>*{padding:4px 0;border-bottom:1px solid var(--b1);font-size:10px}
.ap-kv-row>:first-child{color:var(--t4);font-size:9px;text-transform:uppercase;letter-spacing:.09em}
.ap-kv-row>:last-child{color:var(--t1);font-family:'JetBrains Mono',monospace;font-size:10px}
.rwy-item{margin-bottom:6px}
.rwy-name{font-size:10px;font-weight:600;font-family:'JetBrains Mono',monospace;color:var(--t1);margin-bottom:3px;display:flex;justify-content:space-between}
.rwy-bar{height:5px;background:var(--bg4);border-radius:2px;overflow:hidden}
.rwy-fill{height:100%;background:#4fa3e0;border-radius:2px;transition:width .6s}
.apfl-item{display:grid;grid-template-columns:52px 1fr 44px 44px;border-bottom:1px solid var(--b1);padding:5px 0;gap:3px;align-items:center}
.apfl-cs{font-size:10px;font-weight:700;font-family:'JetBrains Mono',monospace}
.apfl-dest{font-size:10px;color:var(--t2)}
.apfl-time{font-size:10px;font-family:'JetBrains Mono',monospace;color:var(--t1);text-align:right}
.apfl-st{font-size:8px;text-align:right;font-weight:600;letter-spacing:.05em}
.staff-item{display:flex;align-items:center;gap:7px;margin-bottom:5px}
.staff-bar-wrap{flex:1;height:4px;background:var(--bg4);border-radius:2px;overflow:hidden}
.staff-bar-fill{height:100%;border-radius:2px;background:#4fa3e0;transition:width .6s}
#ap-ld{position:absolute;inset:0;background:var(--bg1);z-index:10;display:none;flex-direction:column;align-items:center;justify-content:center;gap:10px;padding:30px;font-size:10px;color:var(--t3);letter-spacing:.1em}
.ap-sp{width:18px;height:18px;border:2px solid var(--bg5);border-top-color:#4fa3e0;border-radius:50%;animation:spin .7s linear infinite}

/* ===== MARITIME CSS ===== */
:root{--sh-cnt:#4a90d9;--sh-tnk:#e8a020;--sh-lng:#c8dc40;--sh-blk:#9060c0;--sh-pas:#40c8a0;--sh-fsh:#60b840;--sh-mil:#e04040;--sh-yht:#d060d0;--sh-cgo:#8090a0;--sh-prt:#d4a020;}
/* Ship panel */
#det-sh-hd{display:none;padding:12px 14px 0}
.sh-name{font-size:14px;font-weight:700;font-family:'JetBrains Mono',monospace;color:var(--acc);letter-spacing:.03em;line-height:1.2;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.sh-mmsi{font-size:9px;color:var(--t3);margin-top:3px;font-family:'JetBrains Mono',monospace;letter-spacing:.06em}
.sh-stats-row{display:flex;gap:5px;margin-top:8px;padding-bottom:10px}
.sh-stat{flex:1;background:var(--bg3);border:1px solid var(--b2);border-radius:3px;padding:5px 4px;text-align:center}
.sh-stat-v{font-size:10px;font-weight:700;font-family:'JetBrains Mono',monospace;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.sh-stat-l{font-size:7px;text-transform:uppercase;letter-spacing:.08em;color:var(--t4);margin-top:1px}
#sh-tab-nav{display:none;border-bottom:1px solid var(--b1);background:var(--bg2);flex-shrink:0}
.sh-tab-btn{flex:1;height:28px;background:transparent;border:none;border-bottom:2px solid transparent;color:var(--t3);font-size:9px;font-weight:600;letter-spacing:.06em;text-transform:uppercase;cursor:pointer;transition:all .1s;font-family:'Inter',sans-serif}
.sh-tab-btn:hover{color:var(--t1);background:var(--adim)}
.sh-tab-btn.on{color:var(--acc);border-bottom-color:var(--acc);background:var(--adim)}
#sh-body{flex:1;overflow:hidden;position:relative;display:none}
.sh-pane{position:absolute;inset:0;overflow-y:auto;display:none;animation:fadein .15s ease}
.sh-pane.on{display:block}
/* Port panel */
#det-port-hd{display:none;padding:12px 14px 0}
.port-nm{font-size:17px;font-weight:800;font-family:'JetBrains Mono',monospace;color:var(--sh-prt);letter-spacing:.04em;line-height:1}
.port-ctry{font-size:9px;color:var(--t3);margin-top:2px}
.port-stats-row{display:flex;gap:5px;margin-top:8px;padding-bottom:10px}
.port-stat{flex:1;background:var(--bg3);border:1px solid var(--b2);border-radius:3px;padding:5px 4px;text-align:center}
.port-stat-v{font-size:11px;font-weight:700;font-family:'JetBrains Mono',monospace;color:var(--sh-prt)}
.port-stat-l{font-size:7px;text-transform:uppercase;letter-spacing:.08em;color:var(--t4);margin-top:1px}
#port-tab-nav{display:none;border-bottom:1px solid var(--b1);background:var(--bg2);flex-shrink:0}
.port-tab-btn{flex:1;height:28px;background:transparent;border:none;border-bottom:2px solid transparent;color:var(--t3);font-size:9px;font-weight:600;letter-spacing:.06em;text-transform:uppercase;cursor:pointer;transition:all .1s;font-family:'Inter',sans-serif}
.port-tab-btn:hover{color:var(--t1);background:var(--adim)}
.port-tab-btn.on{color:var(--sh-prt);border-bottom-color:var(--sh-prt);background:var(--adim)}
#port-body{flex:1;overflow:hidden;position:relative;display:none}
.port-pane{position:absolute;inset:0;overflow-y:auto;display:none;animation:fadein .15s ease}
.port-pane.on{display:block}
/* Anomaly badges */
.ano-badge{display:inline-flex;align-items:center;gap:4px;padding:3px 7px;border-radius:2px;font-size:9px;font-weight:700;letter-spacing:.07em;text-transform:uppercase;margin:2px 2px 2px 0}
.ano-badge.crit{background:rgba(196,64,64,.13);border:1px solid rgba(196,64,64,.35);color:#e06060}
.ano-badge.warn{background:rgba(176,136,40,.13);border:1px solid rgba(176,136,40,.35);color:var(--amb)}
.ano-badge.info{background:rgba(79,163,224,.1);border:1px solid rgba(79,163,224,.3);color:var(--blu)}
/* Loaders for ship/port panels */
#sh-ld{position:absolute;inset:0;background:var(--bg1);z-index:10;display:none;flex-direction:column;align-items:center;justify-content:center;gap:10px;font-size:10px;color:var(--t3);letter-spacing:.1em}
.sh-sp{width:16px;height:16px;border:2px solid var(--bg5);border-top-color:var(--sh-cnt);border-radius:50%;animation:spin .7s linear infinite}
#port-ld{position:absolute;inset:0;background:var(--bg1);z-index:10;display:none;flex-direction:column;align-items:center;justify-content:center;gap:10px;font-size:10px;color:var(--t3);letter-spacing:.1em}
.port-sp{width:16px;height:16px;border:2px solid var(--bg5);border-top-color:var(--sh-prt);border-radius:50%;animation:spin .7s linear infinite}

/* Lightbox */
#lb{position:fixed;inset:0;background:rgba(0,0,0,.96);z-index:9999;display:none;align-items:center;justify-content:center;cursor:pointer}
#lb.on{display:flex}
#lb img{max-width:90vw;max-height:90vh;border-radius:4px;border:1px solid var(--b2)}

/* ===== CONFIG MODAL ===== */
#cfg-modal{position:fixed;inset:0;background:rgba(0,0,0,.82);z-index:10000;display:none;align-items:center;justify-content:center;padding:12px}
#cfg-modal.open{display:flex}
#cfg-box{background:var(--bg2);border:1px solid var(--b2);border-radius:5px;padding:18px;width:430px;max-width:100%;max-height:92vh;overflow-y:auto}
.cfg-title{font-size:13px;font-weight:700;letter-spacing:.08em;color:var(--acc);margin-bottom:6px;font-family:'JetBrains Mono',monospace}
.cfg-label{font-size:9px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--t3);margin:12px 0 4px;display:block}
.cfg-input{width:100%;height:32px;padding:0 9px;background:var(--bg3);border:1px solid var(--b2);color:var(--t1);border-radius:3px;font-size:16px;font-family:'JetBrains Mono',monospace;outline:none}
.cfg-hint{font-size:9px;color:var(--t4);margin-top:3px;line-height:1.5}
.cfg-actions{display:flex;gap:8px;margin-top:16px}
.cfg-save{flex:1;height:34px;background:var(--acc);color:#060606;border:none;border-radius:3px;font-size:12px;font-weight:700;cursor:pointer}
.cfg-close{width:84px;height:34px;background:var(--bg3);color:var(--t2);border:1px solid var(--b2);border-radius:3px;font-size:12px;cursor:pointer}
#cfg-status{font-size:10px;margin-top:8px;color:#5cc85c;display:none}
input,select,textarea{font-size:16px}
#map-gl{position:absolute;inset:0}
#mw.gl #mc{position:absolute;inset:0;pointer-events:none;z-index:2}
#minfo{color:rgba(255,255,255,.4)!important;white-space:pre;z-index:5}
#htip{z-index:6}
.maplibregl-ctrl-attrib{background:rgba(8,8,8,.75)!important;font-size:9px}
.maplibregl-ctrl-attrib,.maplibregl-ctrl-attrib a{color:#999!important}
.maplibregl-ctrl-group{background:#141414!important;border:1px solid #282828}
.maplibregl-ctrl-group button .maplibregl-ctrl-icon{filter:invert(1)}
</style>
<link href="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.css" rel="stylesheet">
<script src="https://unpkg.com/maplibre-gl@4.7.1/dist/maplibre-gl.js"></script>
</head>
<body>
<div id="cfg-modal"><div id="cfg-box">
<div class="cfg-title">CONFIGURATION</div>
<div class="cfg-hint">Les cles sont stockees uniquement dans ton navigateur (localStorage), jamais dans le code GitHub.</div>
<label class="cfg-label">Cle API Anthropic (analyses IA)</label>
<input class="cfg-input" id="cfg-anthr" placeholder="sk-ant-..." autocapitalize="off" autocorrect="off">
<div class="cfg-hint">console.anthropic.com — necessaire pour les onglets IA vol / navire / port / aeroport</div>
<label class="cfg-label">Cle AISStream.io (navires en direct)</label>
<input class="cfg-input" id="cfg-ais" placeholder="cle AISStream" autocapitalize="off" autocorrect="off">
<div class="cfg-hint">aisstream.io — gratuit. Sans cle : navires simules (mode demo)</div>
<label class="cfg-label">Proxy OpenSky (optionnel)</label>
<input class="cfg-input" id="cfg-proxy" placeholder="https://mon-proxy.onrender.com" autocapitalize="off" autocorrect="off">
<div class="cfg-hint">proxy.py sur Render.com — evite les blocages CORS pour les avions live</div>
<label class="cfg-label">Token Cesium Ion (optionnel)</label>
<input class="cfg-input" id="cfg-cesium" placeholder="eyJhbGci..." autocapitalize="off" autocorrect="off">
<div class="cfg-hint">cesium.com/ion — batiments 3D</div>
<div id="cfg-status">Enregistre.</div>
<div class="cfg-actions"><button class="cfg-save" onclick="saveCfg()">Enregistrer</button><button class="cfg-close" onclick="closeCfg()">Fermer</button></div>
</div></div>

<div id="lb" onclick="this.classList.remove('on')"><img id="lb-img" src="" alt=""></div>

<div id="app">

<!-- ===== MENUBAR ===== -->
<div id="menubar">
  <div class="mb-brand">
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2" stroke-linecap="round">
      <ellipse cx="12" cy="12" rx="11" ry="6"/><circle cx="12" cy="12" r="3" fill="#fff" stroke="none"/>
    </svg>
    <div><div class="mb-t">GOD'S EYE</div><div class="mb-s">Aerial & Maritime Intelligence</div></div>
  </div>
  <div class="mb-tabs">
    <button class="mb-tab on" id="tab-2d" onclick="switchView('2d')">Carte 2D</button>
    <button class="mb-tab globe-tab" id="tab-3d" onclick="switchView('3d')">
      <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" style="margin-right:5px"><circle cx="12" cy="12" r="10"/><path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/></svg>Globe 3D
    </button>
  </div>
  <div class="mb-right">
    <div class="mb-badge"><div class="mb-led" id="led" style="animation:blink 2.5s infinite"></div><span id="s-txt">Init...</span></div>
    <div class="mb-badge" id="diag-wrap" style="flex-direction:column;align-items:flex-start;gap:1px;cursor:pointer" onclick="togDiag()">
      <div style="display:flex;align-items:center;gap:5px"><span>ADS-B</span><span style="color:var(--t4);font-size:9px" onclick="togDiag()">Diag</span></div>
      <div id="diag-p" style="display:none;font-size:9px;color:var(--t4)">Src: <span id="src-lbl">-</span><div id="diag-log" style="max-height:60px;overflow-y:auto;line-height:1.6"></div></div>
    </div>
    <button class="mb-btn" onclick="openCfg()">Config</button>
    <button class="mb-btn" id="ref-btn">Actualiser</button>
  </div>
</div>

<!-- ===== CMDBAR (2D) ===== -->
<div id="cmdbar">
  <div class="cg">
    <span class="cg-lbl">Region</span>
    <div id="regions" style="display:flex;gap:2px">
      <button class="rb on" data-r="europe">Europe</button>
      <button class="rb" data-r="france">France</button>
      <button class="rb" data-r="northamerica">Amer. Nord</button>
      <button class="rb" data-r="world">Monde</button>
    </div>
  </div>
  <div class="cg"><span class="cg-lbl">Fond</span><button class="rb bm on" data-bm="dark" onclick="setBasemap('dark',this)">Sombre</button><button class="rb bm" data-bm="light" onclick="setBasemap('light',this)">Clair</button><button class="rb bm" data-bm="osm" onclick="setBasemap('osm',this)">Rues</button><button class="rb bm" data-bm="sat" onclick="setBasemap('sat',this)">Satellite</button></div>
  <div class="cs"><span class="cs-lbl">Total</span><span class="cs-val g" id="sv-tot">-</span></div>
  <div class="cs"><span class="cs-lbl">Militaire</span><span class="cs-val r" id="sv-mil">-</span></div>
  <div class="cs"><span class="cs-lbl">Commercial</span><span class="cs-val" id="sv-com">-</span></div>
  <div class="cs"><span class="cs-lbl">Cargo</span><span class="cs-val a" id="sv-cgo">-</span></div>
  <div class="cs"><span class="cs-lbl">Autres</span><span class="cs-val" id="sv-oth">-</span></div>
  <div style="width:1px;background:var(--b2);height:14px;margin:0 4px;flex-shrink:0"></div>
  <div class="cs"><span class="cs-lbl" style="color:var(--sh-cnt)">Navires</span><span class="cs-val" style="color:var(--sh-cnt)" id="sv-ships">0</span></div>
  <div class="cs"><span class="cs-lbl" style="color:var(--sh-prt)">Ports</span><span class="cs-val" style="color:var(--sh-prt)" id="sv-ports">-</span></div>
  <div class="cg" style="border-right:none">
    <span class="cg-lbl">Maritime</span>
    <button class="rb on" id="btn-ships-tog" onclick="toggleShowShips()">Navires</button>
    <button class="rb on" id="btn-ports-tog" onclick="toggleShowPorts()">Ports</button>
    <button class="rb on" id="btn-zones-tog" onclick="toggleShowZones()">Zones</button>
  </div>
  <div class="cb-time" id="sv-time">--:--:--</div>
</div>

<!-- ===== 2D VIEW ===== -->
<div id="view-2d">
  <!-- Left sidebar — dual mode AERIAL / MARITIME -->
  <div id="sb">
    <!-- Mode switcher -->
    <div id="sb-mode">
      <button class="sb-mode-btn aer on" id="sb-btn-aer" onclick="setSbMode('aerial')">
        <svg width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M17.8 19.2L16 11l3.5-3.5C21 6 21.5 4 21 3c-1-.5-3 0-4.5 1.5L13 8 4.8 6.2c-.5-.1-.9.1-1.1.5l-.3.5c-.2.5-.1 1 .3 1.3L9 12l-2 3H4l-1 1 3 2 2 3 1-1v-3l3-2 3.5 5.3c.3.4.8.5 1.3.3l.5-.2c.4-.3.6-.7.5-1.2z"/></svg>
        Aerien <span id="sb-aer-ct" style="font-size:9px;color:var(--t4)">-</span>
      </button>
      <button class="sb-mode-btn mar" id="sb-btn-mar" onclick="setSbMode('maritime')">
        <svg width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M2 20h20M3 20l2-8h14l2 8M7 12V8l5-4 5 4v4"/></svg>
        Maritime <span id="sb-mar-ct" style="font-size:9px;color:var(--t4)">-</span>
      </button>
      <button class="sb-mode-btn all" id="sb-btn-all" onclick="setSbMode('all')" style="border-top:1px solid var(--b1)">
        <svg width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><rect x="2" y="3" width="20" height="18" rx="2"/><path d="M8 10h8M8 14h5"/></svg>
        Tout <span id="sb-all-ct" style="font-size:9px;color:var(--t4)">-</span>
      </button>
    </div>

    <!-- ===== AERIAL PANE ===== -->
    <div id="sb-aerial">
      <div style="height:26px;flex-shrink:0;background:var(--bg2);border-bottom:1px solid var(--b1);display:flex;align-items:center;justify-content:space-between;padding:0 11px">
        <span style="font-size:9px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--t2)">Vols actifs</span>
        <span style="font-size:10px;color:var(--t4);font-family:'JetBrains Mono',monospace" id="fl-ct">-</span>
      </div>
      <div class="sb-srch"><input type="text" id="s-input" placeholder="Rechercher vol, ICAO, pays..."></div>
      <div class="sb-chips">
        <button class="chip on" data-f="all">Tous</button>
        <button class="chip" data-f="military">Mil.</button>
        <button class="chip" data-f="commercial">Civil</button>
        <button class="chip" data-f="cargo">Cargo</button>
        <button class="chip" data-f="private">Prive</button>
      </div>
      <div class="sb-th"><div></div><div class="sb-th-c">Vol / Pays</div><div class="sb-th-c" style="text-align:right">Alt</div></div>
      <div id="err-box"></div>
      <div id="fl-list" style="flex:1;overflow-y:auto"></div>
      <div id="sb-ft">ADS-B via OpenSky  |  Refresh 15s</div>
    </div>

    <!-- ===== MARITIME PANE ===== -->
    <div id="sb-maritime">
      <!-- Search -->
      <div class="mar-srch"><input type="text" id="mar-input" placeholder="Navire, MMSI, destination..."></div>
      <!-- Type chips -->
      <div class="mar-chips">
        <button class="chip on" data-m="all" onclick="setShipChip(this,'all')">Tous</button>
        <button class="chip" data-m="container" onclick="setShipChip(this,'container')" style="border-color:var(--sh-cnt)">Cont.</button>
        <button class="chip" data-m="tanker" onclick="setShipChip(this,'tanker')" style="border-color:var(--sh-tnk)">Tank.</button>
        <button class="chip" data-m="passenger" onclick="setShipChip(this,'passenger')" style="border-color:var(--sh-pas)">Pass.</button>
        <button class="chip" data-m="military" onclick="setShipChip(this,'military')" style="border-color:var(--sh-mil)">Mil.</button>
      </div>

      <!-- Ships section -->
      <div class="sb-section-hd">
        <span class="sb-section-ht" style="color:var(--sh-cnt)">Navires AIS</span>
        <span class="sb-section-ct" id="sh-list-ct">-</span>
      </div>
      <div id="sh-list" style="flex:0 0 auto;max-height:42%;overflow-y:auto;border-bottom:1px solid var(--b1)"></div>

      <!-- Ports section -->
      <div class="sb-section-hd">
        <span class="sb-section-ht" style="color:var(--sh-prt)">Ports maritimes</span>
        <span class="sb-section-ct" id="port-list-ct">-</span>
      </div>
      <div id="port-list" style="flex:1;overflow-y:auto"></div>

      <div id="sb-ft-mar" style="padding:6px 10px;border-top:1px solid var(--b1);font-size:9px;color:var(--t4);line-height:1.8;flex-shrink:0">
        AIS demo  |  <span id="ais-src-lbl">Mode demo</span>  |  <span id="ano-ct" style="color:var(--red)">0 alertes</span>
      </div>
    </div>

    <!-- ===== ALL / COMBINED PANE ===== -->
    <div id="sb-all" style="display:none;flex-direction:column;flex:1;overflow:hidden;min-height:0">
      <!-- Quick filters row -->
      <div style="padding:5px 7px;border-bottom:1px solid var(--b1);flex-shrink:0;display:flex;gap:3px;align-items:center">
        <input type="text" id="all-input" placeholder="Rechercher tout..." style="flex:1;height:24px;padding:0 8px;background:var(--bg2);border:1px solid var(--b2);color:var(--t1);border-radius:2px;font-size:11px;font-family:'Inter',sans-serif;outline:none">
      </div>
      <div style="display:flex;gap:2px;padding:4px 7px;border-bottom:1px solid var(--b1);flex-shrink:0">
        <button class="chip on" id="all-chip-air" onclick="setAllChip('air',this)">Avions</button>
        <button class="chip on" id="all-chip-ship" onclick="setAllChip('ship',this)">Navires</button>
        <button class="chip on" id="all-chip-port" onclick="setAllChip('port',this)">Ports</button>
      </div>
      <!-- Stats bar -->
      <div style="display:flex;padding:4px 8px;gap:8px;border-bottom:1px solid var(--b1);flex-shrink:0">
        <span style="font-size:9px;color:var(--t3)">Avions: <span id="all-ct-air" style="color:var(--acc);font-family:'JetBrains Mono',monospace;font-weight:700">-</span></span>
        <span style="font-size:9px;color:var(--t3)">Navires: <span id="all-ct-ship" style="color:var(--sh-cnt);font-family:'JetBrains Mono',monospace;font-weight:700">-</span></span>
        <span style="font-size:9px;color:var(--t3)">Ports: <span id="all-ct-port" style="color:var(--sh-prt);font-family:'JetBrains Mono',monospace;font-weight:700">-</span></span>
      </div>
      <!-- Combined unified list -->
      <div id="all-list" style="flex:1;overflow-y:auto"></div>
      <div style="padding:5px 10px;border-top:1px solid var(--b1);font-size:9px;color:var(--t4);flex-shrink:0">
        Vue unifiee — ADS-B + AIS + Ports
      </div>
    </div>
  </div>

  <!-- Map -->
  <div id="mw">
    <div id="map-gl"></div>
    <canvas id="mc"></canvas>
    <div id="minfo"></div>
    <div id="leg">
      <div class="leg-hd">Legende <span class="leg-x" onclick="hideLeg()">x</span></div>
      <div class="leg-sec">Type de vol</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--red)"></div>Militaire</div>
      <div class="leg-row"><div class="leg-dot" style="background:#d0d0d0"></div>Commercial</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--amb)"></div>Cargo / Fret</div>
      <div class="leg-row"><div class="leg-dot" style="background:#8080cc"></div>Prive / Bizjet</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--grn)"></div>Helicoptere</div>
      <div class="leg-row"><div class="leg-dot" style="background:#484848"></div>Inconnu</div>
      <div class="leg-sec">Trajectoire</div>
      <div class="leg-row"><div class="leg-line" style="background:rgba(200,200,200,.28)"></div>Trail recent (gris)</div>
      <div class="leg-row"><div class="leg-line" style="background:var(--grn)"></div>Trajet parcouru (vert)</div>
      <div class="leg-sec">Infrastructures</div>
      <div class="leg-row"><div class="leg-dot" style="background:#4fa3e0;border-radius:1px;width:8px;height:8px"></div>Aeroport (clic = infos)</div>
      <div class="leg-sec">Navires</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--sh-cnt)"></div>Porte-conteneurs</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--sh-tnk)"></div>Petrolier / Tanker</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--sh-pas)"></div>Paquebot</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--sh-mil)"></div>Navire militaire</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--sh-blk)"></div>Vraquier</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--sh-fsh)"></div>Peche</div>
      <div class="leg-sec">Maritime</div>
      <div class="leg-row"><div class="leg-dot" style="background:var(--sh-prt);width:8px;height:8px;transform:rotate(45deg);border-radius:1px"></div>Port (diamant)</div>
      <div class="leg-row"><div class="leg-line" style="background:rgba(220,50,50,.7)"></div>Zone strategique</div>
    </div>
    <button id="leg-show" onclick="showLeg()">Legende</button>
    <div id="htip"></div>
    <div id="loader"><div class="ld-sp"></div><div class="ld-tx">GOD'S EYE - INITIALISATION</div></div>
  </div>
</div>

<!-- ===== 3D GLOBE VIEW ===== -->
<div id="view-3d">

  <!-- Globe loader -->
  <div id="globe-loader">
    <div class="gl-sp"></div>
    <div class="gl-title">GOD'S EYE</div>
    <div class="gl-sub">Chargement Globe 3D...</div>
  </div>

  <!-- Cesium container -->
  <div id="cesiumContainer"></div>

  <!-- Globe notification -->
  <div id="globe-notif"></div>

  <!-- Measure hint -->
  <div id="measure-hint">Mode mesure — Cliquez 2 points sur le globe</div>

  <!-- Globe controls toggle -->
  <button id="gctrl-toggle" onclick="toggleGCtrl()">&#9776;</button>

  <!-- Globe controls panel -->
  <div id="gctrl">
    <div class="gc-search">
      <div class="gc-search-row">
        <input type="text" id="gc-search-input" placeholder="Rechercher lieu, aéroport...">
        <button id="gc-search-btn" onclick="globeSearch()">&#8594;</button>
      </div>
    </div>
    <div id="gc-search-results"></div>
    <div class="gc-scroll">
      <div class="gc-section">
        <div class="gc-sec-title">Fond de carte</div>
        <div class="img-grid">
          <button class="img-btn on" onclick="switchImagery('satellite',this)">Satellite</button>
          <button class="img-btn" onclick="switchImagery('streets',this)">Routes</button>
          <button class="img-btn" onclick="switchImagery('hybrid',this)">Hybride</button>
          <button class="img-btn" onclick="switchImagery('night',this)">Nuit NASA</button>
          <button class="img-btn" onclick="switchImagery('topo',this)">Relief</button>
          <button class="img-btn" onclick="switchImagery('dark',this)">Dark</button>
        </div>
      </div>
      <div class="gc-section">
        <div class="gc-sec-title">Couches</div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:var(--t1)"></div>Vols en direct</div><label class="tog-sw"><input type="checkbox" id="tog-flights" checked onchange="toggleGlobeFlights(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:var(--blu)"></div>Aéroports</div><label class="tog-sw"><input type="checkbox" id="tog-airports" checked onchange="toggleGlobeAirports(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:var(--sh-cnt)"></div>Navires AIS</div><label class="tog-sw"><input type="checkbox" id="tog-globe-ships" checked onchange="toggleGlobeShips(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:var(--sh-prt)"></div>Ports maritimes</div><label class="tog-sw"><input type="checkbox" id="tog-globe-ports" checked onchange="toggleGlobePorts(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:#ff4444"></div>Zones stratégiques</div><label class="tog-sw"><input type="checkbox" id="tog-globe-zones" checked onchange="toggleGlobeZones(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:var(--grn)"></div>Atmosphère</div><label class="tog-sw"><input type="checkbox" id="tog-atmos" checked onchange="toggleAtmos(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:var(--amb)"></div>Éclairage solaire</div><label class="tog-sw"><input type="checkbox" id="tog-light" onchange="toggleSunLight(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:var(--pur)"></div>Bâtiments 3D (Ion)</div><label class="tog-sw"><input type="checkbox" id="tog-buildings" onchange="toggleBuildings(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:#607090"></div>Frontières</div><label class="tog-sw"><input type="checkbox" id="tog-borders" onchange="toggleBorders(this.checked)"><div class="tog-slider"></div></label></div>
        <div class="tog-row"><div class="tog-lbl"><div class="tog-dot" style="background:#4a7090"></div>Nuages NASA</div><label class="tog-sw"><input type="checkbox" id="tog-clouds" onchange="toggleClouds(this.checked)"><div class="tog-slider"></div></label></div>
      </div>
      <div class="gc-section">
        <div class="gc-sec-title">Filtres vols</div>
        <div class="fchip-row">
          <button class="fchip on" data-t="all" onclick="setGlobeTypeFilter('all',this)">Tous</button>
          <button class="fchip" data-t="military" onclick="setGlobeTypeFilter('military',this)">Mil.</button>
          <button class="fchip" data-t="commercial" onclick="setGlobeTypeFilter('commercial',this)">Civil</button>
          <button class="fchip" data-t="cargo" onclick="setGlobeTypeFilter('cargo',this)">Cargo</button>
          <button class="fchip" data-t="private" onclick="setGlobeTypeFilter('private',this)">Privé</button>
        </div>
        <div class="range-row">
          <div class="range-lbl"><span>Altitude min</span><span id="galt-min-lbl">0 ft</span></div>
          <input type="range" id="galt-min" min="0" max="45000" value="0" oninput="updateGlobeAltFilter()">
        </div>
        <div class="range-row">
          <div class="range-lbl"><span>Altitude max</span><span id="galt-max-lbl">45 000 ft</span></div>
          <input type="range" id="galt-max" min="0" max="45000" value="45000" oninput="updateGlobeAltFilter()">
        </div>
      </div>
      <div class="gc-section">
        <div class="gc-sec-title">Statistiques</div>
        <div class="gc-stats">
          <div class="gc-stat"><div class="gc-stat-v" id="gc-tot">-</div><div class="gc-stat-l">Vols visibles</div></div>
          <div class="gc-stat"><div class="gc-stat-v r" id="gc-mil">-</div><div class="gc-stat-l">Militaires</div></div>
          <div class="gc-stat"><div class="gc-stat-v b" id="gc-ap">-</div><div class="gc-stat-l">Aéroports</div></div>
          <div class="gc-stat"><div class="gc-stat-v" style="color:var(--sh-cnt)" id="gc-ships">-</div><div class="gc-stat-l">Navires</div></div>
          <div class="gc-stat"><div class="gc-stat-v" id="gc-upd" style="font-size:9px">-</div><div class="gc-stat-l">Mise à jour</div></div>
          <div class="gc-stat"><div class="gc-stat-v" style="color:var(--sh-prt)" id="gc-ports">-</div><div class="gc-stat-l">Ports</div></div>
        </div>
      </div>
      <div class="gc-section">
        <div class="gc-sec-title">Outils</div>
        <div class="tool-grid">
          <button class="tool-btn" id="btn-measure" onclick="toggleMeasure()">Mesure</button>
          <button class="tool-btn" onclick="globeFlyHome()">Vue globale</button>
          <button class="tool-btn" onclick="globeScreenshot()">Capture</button>
          <button class="tool-btn" onclick="openStreetView()">Street View</button>
          <button class="tool-btn" onclick="setDayMode()">Mode jour</button>
          <button class="tool-btn" onclick="setNightMode()">Mode nuit</button>
        </div>
      </div>
      <div class="gc-section">
        <div class="gc-sec-title">Villes</div>
        <div class="city-grid">
          <button class="city-btn" onclick="flyToCity('Paris',48.8566,2.3522)">Paris</button>
          <button class="city-btn" onclick="flyToCity('New York',40.7128,-74.006)">New York</button>
          <button class="city-btn" onclick="flyToCity('Dubai',25.2048,55.2708)">Dubai</button>
          <button class="city-btn" onclick="flyToCity('Tokyo',35.6762,139.6503)">Tokyo</button>
          <button class="city-btn" onclick="flyToCity('London',51.5074,-0.1278)">London</button>
          <button class="city-btn" onclick="flyToCity('Sydney',-33.8688,151.2093)">Sydney</button>
        </div>
      </div>
      <div class="gc-section">
        <div class="gc-sec-title">AISStream.io (navires live)</div>
        <div style="font-size:9px;color:var(--t4);line-height:1.6;margin-bottom:4px">Clé gratuite sur <a href="https://aisstream.io" target="_blank" style="color:var(--sh-cnt)">aisstream.io</a></div>
        <input type="text" class="token-input" id="ais-key" placeholder="Votre clé API AISStream...">
        <button class="apply-btn" onclick="connectAISStream()">Connecter AIS live</button>
        <div id="ais-status" style="font-size:9px;color:var(--t4);margin-top:3px">Mode démo actif</div>
      </div>
        <div style="font-size:9px;color:var(--t4);line-height:1.6;margin-bottom:4px">Token gratuit sur <a href="https://cesium.com/ion" target="_blank" style="color:var(--blu)">cesium.com</a></div>
        <input type="text" class="token-input" id="ion-token" placeholder="eyJhbGci...">
        <button class="apply-btn" onclick="applyIonToken()">Appliquer</button>
        <div id="ion-status" style="font-size:9px;color:var(--t4);margin-top:3px"></div>
      </div>
    </div>
  </div>

  <!-- Floating nav buttons -->
  <div id="globe-nav">
    <button class="gnav-btn" onclick="gViewer&&gViewer.camera.zoomIn(400000)" title="Zoom +">+</button>
    <button class="gnav-btn" onclick="gViewer&&gViewer.camera.zoomOut(400000)" title="Zoom -">&#8722;</button>
    <button class="gnav-btn" onclick="tiltCamera(-10)" title="Incliner haut">&#9651;</button>
    <button class="gnav-btn" onclick="tiltCamera(10)" title="Incliner bas">&#9661;</button>
    <div id="compass-wrap" onclick="resetNorth()" title="Orienter nord">
      <div id="compass-ring">
        <div style="text-align:center"><div class="c-n">N</div><div style="font-size:7px;color:var(--t3);font-family:'JetBrains Mono',monospace">S</div></div>
      </div>
    </div>
  </div>

  <!-- Globe HUD -->
  <div id="globe-hud">
    <div class="hud-item">LAT<span id="hud-lat">--</span></div>
    <div class="hud-item">LON<span id="hud-lon">--</span></div>
    <div class="hud-item">ALT<span id="hud-cam-alt">--</span></div>
    <div class="hud-item">FPS<span id="hud-fps">--</span></div>
    <div class="hud-item" style="border:none">SRC<span id="hud-src">-</span></div>
    <div id="hud-scale-wrap">
      <div id="hud-scale-line"></div>
      <div id="hud-scale-lbl">-</div>
    </div>
  </div>
</div><!-- /view-3d -->

<!-- ===== SHARED RIGHT DETAIL PANEL ===== -->
<div id="det-tab" onclick="toggleDet()">
  <div class="det-tab-dot"></div>
  <div class="det-tab-txt">Infos vol</div>
  <div class="det-tab-dot"></div>
</div>

<div id="det">
  <div id="det-hd">
    <div id="det-empty">
      <div id="det-empty-icon">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="#3a3a3a" stroke-width="2">
          <path d="M17.8 19.2L16 11l3.5-3.5C21 6 21.5 4 21 3c-1-.5-3 0-4.5 1.5L13 8 4.8 6.2c-.5-.1-.9.1-1.1.5l-.3.5c-.2.5-.1 1 .3 1.3L9 12l-2 3H4l-1 1 3 2 2 3 1-1v-3l3-2 3.5 5.3c.3.4.8.5 1.3.3l.5-.2c.4-.3.6-.7.5-1.2z"/>
        </svg>
      </div>
      <div id="det-empty-title">Aucun vol selectionne</div>
      <div id="det-empty-sub">Cliquez sur un avion<br>sur la carte ou le globe</div>
    </div>
    <div id="det-flight">
      <div style="display:flex;align-items:flex-start;justify-content:space-between">
        <div>
          <div style="display:flex;align-items:center;gap:7px">
            <span id="det-cc" style="font-size:11px;font-family:'JetBrains Mono',monospace;color:var(--t3);font-weight:700;min-width:22px"></span>
            <span id="det-cs">-</span>
          </div>
          <div id="det-meta">
            <span id="det-badge" class="d-badge">-</span>
            <span id="det-cy" style="font-size:9px;color:var(--t3)">-</span>
          </div>
        </div>
        <button id="det-cls" style="width:26px;height:26px;background:var(--bg3);border:1px solid var(--b2);color:var(--t2);cursor:pointer;border-radius:3px;font-size:14px;display:flex;align-items:center;justify-content:center;flex-shrink:0;font-family:sans-serif">x</button>
      </div>
    </div>
    <div id="tab-nav">
      <button class="tab-btn on" data-tab="route">Route</button>
      <button class="tab-btn" data-tab="live">Live</button>
      <button class="tab-btn" data-tab="ia">IA</button>
      <button class="tab-btn" data-tab="data">Donnees</button>
    </div>
  </div>
  <div id="det-body">
    <div class="tab-pane on" id="tab-route">
      <div class="pane-pad">
        <div class="pane-lbl">Route du vol</div>
        <div class="route">
          <div class="ap"><div class="ap-code" id="dep-c">???</div><div class="ap-city" id="dep-n">Depart</div><div class="ap-time" id="dep-t">--:--</div></div>
          <div class="r-mid">
            <div class="r-fn" id="fnum">-</div>
            <div class="pb-wrap"><div class="pb-fill" id="pb" style="width:0%"><span class="pb-arrow">></span></div></div>
            <div class="pb-pct" id="pb-pct">-</div>
          </div>
          <div class="ap"><div class="ap-code" id="arr-c">???</div><div class="ap-city" id="arr-n">Arrivee</div><div class="ap-time" id="arr-t">--:--</div></div>
        </div>
        <div class="pane-div"></div>
        <div class="pane-lbl">Chronometre de vol</div>
        <div class="tg">
          <div class="tc"><div class="tc-l">Ecoule</div><div class="tc-v" id="t-el">-</div></div>
          <div class="tc"><div class="tc-l">Total</div><div class="tc-v" id="t-tot">-</div></div>
          <div class="tc"><div class="tc-l">Restant</div><div class="tc-v" id="t-rem">-</div></div>
          <div class="tc"><div class="tc-l">ETA local</div><div class="tc-v hl" id="t-eta">-</div></div>
        </div>
        <div class="pane-div"></div>
        <div class="pane-lbl">Infos appareil (via IA)</div>
        <div id="route-ai-info" style="font-size:10px;color:var(--t3);font-style:italic">Active l'onglet IA pour charger les infos...</div>
      </div>
    </div>
    <div class="tab-pane" id="tab-live">
      <div class="pane-pad">
        <div class="pane-lbl">Trajectoire temps reel</div>
        <canvas id="zc" height="108"></canvas>
        <div class="z-hint">Gris = trail recent   |   Vert = trajet parcouru</div>
        <div class="pane-lbl">Donnees en direct</div>
        <div class="gg">
          <div class="gc"><div class="gc-l">Altitude</div><div class="gc-r"><span class="gc-v" id="g-alt">-</span><span class="gc-u">m</span></div><div class="gc-tr"><div class="gc-b" id="gb-alt" style="width:0%"></div></div></div>
          <div class="gc"><div class="gc-l">Vitesse sol</div><div class="gc-r"><span class="gc-v" id="g-spd">-</span><span class="gc-u">km/h</span></div><div class="gc-tr"><div class="gc-b" id="gb-spd" style="width:0%"></div></div></div>
          <div class="gc"><div class="gc-l">Cap</div><div class="gc-r"><span class="gc-v" id="g-hdg">-</span><span class="gc-u">deg</span></div><div class="gc-tr"><div class="gc-b a" id="gb-hdg" style="width:0%"></div></div></div>
          <div class="gc"><div class="gc-l">Taux vertical</div><div class="gc-r"><span id="vr-ar" style="font-size:14px;margin-right:2px;line-height:1">-</span><span class="gc-v" id="g-vr">-</span><span class="gc-u">m/s</span></div><div class="gc-tr"><div class="gc-b" id="gb-vr" style="width:50%"></div></div></div>
        </div>
      </div>
    </div>
    <div class="tab-pane" id="tab-ia">
      <div class="pane-pad">
        <div id="ai-trig" onclick="togAI()">
          <div class="ai-led"></div><span class="ai-hl">Analyse par IA</span><span class="ai-arr" id="ai-arr">Lancer v</span>
        </div>
        <div id="ai-bd">
          <div id="ai-ld"><div class="ai-sp"></div><span id="ai-lt">Analyse en cours...</span></div>
          <div id="ai-sum"></div>
          <table class="ai-tbl" id="ai-tbl"></table>
          <div id="ph-wr"><div class="ph-l">Photos de l'appareil</div><div id="ph-gr"></div></div>
        </div>
      </div>
    </div>
    <div class="tab-pane" id="tab-data">
      <div class="pane-pad">
        <div class="pane-lbl">Donnees ADS-B brutes</div>
        <table class="raw-tbl" id="raw-rows"></table>
        <a class="ext-link" id="fr24-a" href="#" target="_blank">Voir sur FlightRadar24</a>
      </div>
    </div>
  </div>

  <!-- Airport panel (shared) -->
  <div id="det-ap-hd">
    <div style="display:flex;align-items:flex-start;justify-content:space-between">
      <div>
        <div class="ap-iata" id="ap-iata">---</div>
        <div class="ap-fullname" id="ap-fullname">-</div>
        <div class="ap-loc" id="ap-loc">-</div>
      </div>
      <button id="det-ap-cls" style="width:26px;height:26px;background:var(--bg3);border:1px solid var(--b2);color:var(--t2);cursor:pointer;border-radius:3px;font-size:14px;display:flex;align-items:center;justify-content:center;flex-shrink:0;font-family:sans-serif">x</button>
    </div>
    <div class="ap-stats-row">
      <div class="ap-stat"><div class="ap-stat-v" id="ap-pax">-</div><div class="ap-stat-l">Pax/an</div></div>
      <div class="ap-stat"><div class="ap-stat-v" id="ap-rwy">-</div><div class="ap-stat-l">Pistes</div></div>
      <div class="ap-stat"><div class="ap-stat-v" id="ap-live-ct">-</div><div class="ap-stat-l">En direct</div></div>
    </div>
  </div>
  <div id="ap-tab-nav">
    <button class="ap-tab-btn on" data-aptab="overview">Apercu</button>
    <button class="ap-tab-btn" data-aptab="flights">Vols live</button>
    <button class="ap-tab-btn" data-aptab="ia">Analyse IA</button>
  </div>
  <div id="ap-body">
    <div id="ap-ld"><div class="ap-sp"></div><span>ANALYSE EN COURS...</span></div>
    <div class="ap-pane on" id="aptab-overview">
      <div class="ap-section"><div class="ap-sec-title">Identification</div><div class="ap-kv" id="ap-ident"></div></div>
      <div class="ap-section"><div class="ap-sec-title">Direction &amp; exploitation</div><div class="ap-kv" id="ap-mgmt"></div></div>
      <div class="ap-section"><div class="ap-sec-title">Actionnariat</div><div id="ap-shareholders" style="font-size:10px;color:var(--t2);line-height:1.7"></div></div>
      <div class="ap-section"><div class="ap-sec-title">Pistes d'atterrissage</div><div id="ap-runways"></div></div>
      <div class="ap-section"><div class="ap-sec-title">Personnel (estimation)</div><div id="ap-staff"></div></div>
      <div class="ap-section" style="border:none"><div class="ap-sec-title">Compagnies principales</div><div id="ap-airlines" style="font-size:10px;color:var(--t2);line-height:1.8"></div></div>
    </div>
    <div class="ap-pane" id="aptab-flights">
      <div style="padding:8px 13px;border-bottom:1px solid var(--b1)">
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:4px">
          <div style="background:var(--bg2);border:1px solid var(--b1);border-radius:3px;padding:6px 9px"><div style="font-size:8px;text-transform:uppercase;letter-spacing:.12em;color:var(--t4);margin-bottom:4px">Departs imminents</div><div id="ap-dep-list"></div></div>
          <div style="background:var(--bg2);border:1px solid var(--b1);border-radius:3px;padding:6px 9px"><div style="font-size:8px;text-transform:uppercase;letter-spacing:.12em;color:var(--t4);margin-bottom:4px">Arrivees imminentes</div><div id="ap-arr-list"></div></div>
        </div>
      </div>
      <div style="padding:8px 13px"><div style="font-size:8px;text-transform:uppercase;letter-spacing:.12em;color:var(--t4);margin-bottom:6px">Trafic ADS-B en direct</div><div id="ap-live-list"></div></div>
    </div>
    <div class="ap-pane" id="aptab-ia">
      <div style="padding:12px 13px">
        <div id="ap-ai-summary" style="font-size:11px;color:var(--t2);line-height:1.7;white-space:pre-wrap;margin-bottom:10px"></div>
        <div id="ap-ai-extra"></div>
      </div>
    </div>
  </div>

  <!-- ===== SHIP PANEL ===== -->
  <div id="det-sh-hd">
    <div style="display:flex;align-items:flex-start;justify-content:space-between;gap:8px">
      <div style="min-width:0;flex:1">
        <div class="sh-name" id="sh-name">---</div>
        <div class="sh-mmsi" id="sh-mmsi">MMSI ---</div>
      </div>
      <button id="det-sh-cls" style="width:26px;height:26px;flex-shrink:0;background:var(--bg3);border:1px solid var(--b2);color:var(--t2);cursor:pointer;border-radius:3px;font-size:14px;display:flex;align-items:center;justify-content:center;font-family:sans-serif">x</button>
    </div>
    <div style="margin-top:6px;margin-bottom:6px">
      <span id="sh-badge" style="display:inline-flex;align-items:center;height:17px;padding:0 6px;border-radius:2px;font-size:8px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;border:1px solid transparent">-</span>
      <span id="sh-flag" style="font-size:9px;color:var(--t3);margin-left:6px">-</span>
    </div>
    <div class="sh-stats-row">
      <div class="sh-stat"><div class="sh-stat-v" id="sh-spd" style="color:var(--sh-cnt)">-</div><div class="sh-stat-l">Vitesse</div></div>
      <div class="sh-stat"><div class="sh-stat-v" id="sh-hdg">-</div><div class="sh-stat-l">Cap</div></div>
      <div class="sh-stat"><div class="sh-stat-v" id="sh-stat-nav" style="font-size:8px">-</div><div class="sh-stat-l">Statut</div></div>
    </div>
  </div>
  <div id="sh-tab-nav">
    <button class="sh-tab-btn on" data-shtab="info">Identite</button>
    <button class="sh-tab-btn" data-shtab="nav">Navigation</button>
    <button class="sh-tab-btn" data-shtab="voyage">Voyage</button>
    <button class="sh-tab-btn" data-shtab="ia">IA</button>
    <button class="sh-tab-btn" data-shtab="anomalies">Alertes</button>
  </div>
  <div id="sh-body">
    <div id="sh-ld"><div class="sh-sp"></div><span>ANALYSE EN COURS...</span></div>
    <!-- TAB: IDENTITE -->
    <div class="sh-pane on" id="shtab-info">
      <div class="ap-section"><div class="ap-sec-title">Identite du navire</div><div class="ap-kv" id="sh-ident"></div></div>
      <div class="ap-section"><div class="ap-sec-title">Caracteristiques</div><div class="ap-kv" id="sh-chars"></div></div>
      <div class="ap-section" style="border:none"><div class="ap-sec-title">Liens externes</div>
        <a class="ext-link" id="sh-mt-link" href="#" target="_blank" style="margin-bottom:4px">Voir sur MarineTraffic</a>
        <a class="ext-link" id="sh-vf-link" href="#" target="_blank">Voir sur VesselFinder</a>
      </div>
    </div>
    <!-- TAB: NAVIGATION -->
    <div class="sh-pane" id="shtab-nav">
      <div class="ap-section"><div class="ap-sec-title">Position & mouvement</div><div class="ap-kv" id="sh-nav-data"></div></div>
      <div class="ap-section" style="border:none">
        <div class="ap-sec-title">Trajectoire recente</div>
        <canvas id="sh-zc" height="110" style="width:100%;border:1px solid var(--b1);border-radius:3px;background:#060606;display:block;margin-top:4px"></canvas>
        <div style="font-size:8px;color:var(--t4);text-align:center;margin-top:3px">Bleu = trail AIS   |   Blanc = position actuelle</div>
      </div>
    </div>
    <!-- TAB: VOYAGE -->
    <div class="sh-pane" id="shtab-voyage">
      <div class="ap-section">
        <div class="ap-sec-title">Route en cours</div>
        <div class="route" style="margin-bottom:8px">
          <div class="ap" style="text-align:center;flex:1"><div class="ap-code" id="sh-dep" style="font-size:16px">???</div><div class="ap-city" id="sh-dep-name">Départ</div></div>
          <div class="r-mid" style="flex:2;padding:0 7px;display:flex;flex-direction:column;justify-content:center">
            <div style="font-size:9px;font-family:'JetBrains Mono',monospace;text-align:center;color:var(--t3);margin-bottom:5px" id="sh-progress-lbl">En route</div>
            <div class="pb-wrap"><div class="pb-fill" id="sh-pb" style="width:0%"><span class="pb-arrow">></span></div></div>
          </div>
          <div class="ap" style="text-align:center;flex:1"><div class="ap-code" id="sh-dest" style="font-size:16px">???</div><div class="ap-city" id="sh-dest-name">Destination</div></div>
        </div>
        <div class="ap-kv" id="sh-voyage-data"></div>
      </div>
      <div class="ap-section" style="border:none"><div class="ap-sec-title">Derniers points AIS</div><div id="sh-trail-list" style="font-size:10px;color:var(--t3)">-</div></div>
    </div>
    <!-- TAB: IA -->
    <div class="sh-pane" id="shtab-ia">
      <div class="pane-pad">
        <div id="sh-ai-trig" onclick="togShipAI()" style="display:flex;align-items:center;gap:8px;padding:11px 13px;background:var(--bg2);border:1px solid var(--b2);border-radius:3px;cursor:pointer;margin-bottom:10px">
          <div style="width:6px;height:6px;border-radius:50%;background:var(--pur);animation:blink 2s infinite;flex-shrink:0"></div>
          <span style="font-weight:600;font-size:11px;color:var(--t1)">Analyse navire par IA</span>
          <span id="sh-ai-arr" style="margin-left:auto;font-size:10px;color:var(--t3);font-family:'JetBrains Mono',monospace">Lancer v</span>
        </div>
        <div id="sh-ai-bd" style="display:none">
          <div id="sh-ai-ld" style="display:none;align-items:center;gap:7px;font-size:10px;color:var(--t3);margin-bottom:8px"><div class="sh-sp"></div><span>Analyse en cours...</span></div>
          <div id="sh-ai-sum" style="font-size:11px;color:var(--t2);line-height:1.7;white-space:pre-wrap;margin-bottom:8px"></div>
          <table class="ai-tbl" id="sh-ai-tbl"></table>
        </div>
      </div>
    </div>
    <!-- TAB: ANOMALIES -->
    <div class="sh-pane" id="shtab-anomalies">
      <div class="pane-pad">
        <div class="pane-lbl">Alertes detectees</div>
        <div id="sh-anomalies-list"></div>
        <div class="pane-div"></div>
        <div class="pane-lbl">Zones strategiques proches</div>
        <div id="sh-zones-list" style="font-size:10px;color:var(--t3)">Aucune zone strategique a proximite</div>
      </div>
    </div>
  </div>

  <!-- ===== PORT PANEL ===== -->
  <div id="det-port-hd">
    <div style="display:flex;align-items:flex-start;justify-content:space-between">
      <div>
        <div class="port-nm" id="port-name">---</div>
        <div class="port-ctry" id="port-ctry">-</div>
        <span id="port-badge" style="display:inline-flex;align-items:center;height:16px;padding:0 6px;border-radius:2px;font-size:8px;font-weight:700;letter-spacing:.1em;text-transform:uppercase;border:1px solid transparent;margin-top:5px">-</span>
      </div>
      <button id="det-port-cls" style="width:26px;height:26px;background:var(--bg3);border:1px solid var(--b2);color:var(--t2);cursor:pointer;border-radius:3px;font-size:14px;display:flex;align-items:center;justify-content:center;flex-shrink:0;font-family:sans-serif">x</button>
    </div>
    <div class="port-stats-row">
      <div class="port-stat"><div class="port-stat-v" id="port-cap">-</div><div class="port-stat-l">Capacite</div></div>
      <div class="port-stat"><div class="port-stat-v" id="port-term">-</div><div class="port-stat-l">Terminaux</div></div>
      <div class="port-stat"><div class="port-stat-v" id="port-depth">-</div><div class="port-stat-l">Tirant</div></div>
    </div>
  </div>
  <div id="port-tab-nav">
    <button class="port-tab-btn on" data-ptab="overview">Apercu</button>
    <button class="port-tab-btn" data-ptab="traffic">Trafic</button>
    <button class="port-tab-btn" data-ptab="ia">Analyse IA</button>
  </div>
  <div id="port-body">
    <div id="port-ld"><div class="port-sp"></div><span>ANALYSE EN COURS...</span></div>
    <div class="port-pane on" id="porttab-overview">
      <div class="ap-section"><div class="ap-sec-title">Identification</div><div class="ap-kv" id="port-ident"></div></div>
      <div class="ap-section"><div class="ap-sec-title">Infrastructure</div><div class="ap-kv" id="port-infra"></div></div>
      <div class="ap-section" style="border:none"><div class="ap-sec-title">Resume</div><div id="port-summary" style="font-size:11px;color:var(--t2);line-height:1.7"></div></div>
    </div>
    <div class="port-pane" id="porttab-traffic">
      <div class="ap-section"><div class="ap-sec-title">Navires AIS detectes a proximite</div><div id="port-traffic-list" style="font-size:10px;color:var(--t3);line-height:1.8">Chargement...</div></div>
      <div class="ap-section" style="border:none"><div class="ap-sec-title">Statistiques trafic</div><div class="ap-kv" id="port-traffic-stats"></div></div>
    </div>
    <div class="port-pane" id="porttab-ia">
      <div style="padding:12px 13px">
        <div id="port-ai-sum" style="font-size:11px;color:var(--t2);line-height:1.7;white-space:pre-wrap;margin-bottom:10px">Chargement de l'analyse IA...</div>
        <table class="ai-tbl" id="port-ai-tbl"></table>
      </div>
    </div>
  </div>
</div>
<script>
// ============================================================
// GOD'S EYE v3.0 — COMBINED 2D + 3D INTELLIGENCE PLATFORM
// ============================================================

// ===== CONFIG (localStorage) =====
const CFG_KEY="godseye_cfg_v1";
const AI_MODEL="claude-sonnet-5-5";
let CFG={anthropic:"",aisStream:"",proxy:"",cesium:""};
try{const s=localStorage.getItem(CFG_KEY);if(s)CFG=Object.assign(CFG,JSON.parse(s));}catch(e){}
function saveCfgLS(){try{localStorage.setItem(CFG_KEY,JSON.stringify(CFG));}catch(e){}}
function openCfg(){
  document.getElementById("cfg-anthr").value=CFG.anthropic;
  document.getElementById("cfg-ais").value=CFG.aisStream;
  document.getElementById("cfg-proxy").value=CFG.proxy;
  document.getElementById("cfg-cesium").value=CFG.cesium;
  document.getElementById("cfg-modal").classList.add("open");
}
function closeCfg(){document.getElementById("cfg-modal").classList.remove("open");}
function saveCfg(){
  CFG.anthropic=document.getElementById("cfg-anthr").value.trim();
  CFG.aisStream=document.getElementById("cfg-ais").value.trim();
  CFG.proxy=document.getElementById("cfg-proxy").value.trim().replace(/\/$/,"");
  CFG.cesium=document.getElementById("cfg-cesium").value.trim();
  saveCfgLS();
  const ak=document.getElementById("ais-key");if(ak)ak.value=CFG.aisStream;
  const it=document.getElementById("ion-token");if(it)it.value=CFG.cesium;
  if(CFG.cesium&&typeof Cesium!=="undefined")Cesium.Ion.defaultAccessToken=CFG.cesium;
  const st=document.getElementById("cfg-status");st.style.display="block";setTimeout(()=>st.style.display="none",2500);
  if(CFG.aisStream)connectAISStream();
  loadFlights();
}
function aiHeaders(){
  if(!CFG.anthropic)throw new Error("Cle API Anthropic manquante - ouvre Config");
  return {"Content-Type":"application/json","x-api-key":CFG.anthropic,"anthropic-version":"2023-06-01","anthropic-dangerous-direct-browser-access":"true"};
}
document.addEventListener("keydown",e=>{if(e.key==="Escape")closeCfg();});


// ===== SHARED DATA =====
const AIRPORTS=[
  {iata:"CDG",icao:"LFPG",name:"Charles de Gaulle",city:"Paris",country:"France",lat:49.0097,lon:2.5479,size:1},
  {iata:"ORY",icao:"LFPO",name:"Paris-Orly",city:"Paris",country:"France",lat:48.7233,lon:2.3794,size:1},
  {iata:"LYS",icao:"LFLL",name:"Lyon-Saint Exupery",city:"Lyon",country:"France",lat:45.7256,lon:5.0811,size:2},
  {iata:"NCE",icao:"LFMN",name:"Nice Cote d'Azur",city:"Nice",country:"France",lat:43.6584,lon:7.2159,size:2},
  {iata:"MRS",icao:"LFML",name:"Marseille Provence",city:"Marseille",country:"France",lat:43.4393,lon:5.2214,size:2},
  {iata:"BOD",icao:"LFBD",name:"Bordeaux-Merignac",city:"Bordeaux",country:"France",lat:44.8283,lon:-0.7155,size:2},
  {iata:"TLS",icao:"LFBO",name:"Toulouse-Blagnac",city:"Toulouse",country:"France",lat:43.6293,lon:1.3638,size:2},
  {iata:"NTE",icao:"LFRS",name:"Nantes Atlantique",city:"Nantes",country:"France",lat:47.1532,lon:-1.6108,size:2},
  {iata:"LHR",icao:"EGLL",name:"London Heathrow",city:"Londres",country:"Royaume-Uni",lat:51.4775,lon:-0.4614,size:1},
  {iata:"LGW",icao:"EGKK",name:"London Gatwick",city:"Londres",country:"Royaume-Uni",lat:51.1481,lon:-0.1903,size:1},
  {iata:"STN",icao:"EGSS",name:"London Stansted",city:"Londres",country:"Royaume-Uni",lat:51.885,lon:0.235,size:2},
  {iata:"AMS",icao:"EHAM",name:"Amsterdam Schiphol",city:"Amsterdam",country:"Pays-Bas",lat:52.3086,lon:4.7639,size:1},
  {iata:"FRA",icao:"EDDF",name:"Frankfurt am Main",city:"Francfort",country:"Allemagne",lat:50.0379,lon:8.5622,size:1},
  {iata:"MUC",icao:"EDDM",name:"Munich Franz Josef Strauss",city:"Munich",country:"Allemagne",lat:48.3537,lon:11.775,size:1},
  {iata:"DUS",icao:"EDDL",name:"Dusseldorf",city:"Dusseldorf",country:"Allemagne",lat:51.2895,lon:6.7668,size:2},
  {iata:"HAM",icao:"EDDH",name:"Hamburg",city:"Hambourg",country:"Allemagne",lat:53.6304,lon:9.9882,size:2},
  {iata:"MAD",icao:"LEMD",name:"Madrid Barajas",city:"Madrid",country:"Espagne",lat:40.4936,lon:-3.5668,size:1},
  {iata:"BCN",icao:"LEBL",name:"Barcelona El Prat",city:"Barcelone",country:"Espagne",lat:41.2971,lon:2.0785,size:1},
  {iata:"FCO",icao:"LIRF",name:"Roma Fiumicino",city:"Rome",country:"Italie",lat:41.8003,lon:12.2389,size:1},
  {iata:"MXP",icao:"LIMC",name:"Milan Malpensa",city:"Milan",country:"Italie",lat:45.6306,lon:8.7281,size:1},
  {iata:"VIE",icao:"LOWW",name:"Vienna International",city:"Vienne",country:"Autriche",lat:48.1103,lon:16.5697,size:1},
  {iata:"ZRH",icao:"LSZH",name:"Zurich Airport",city:"Zurich",country:"Suisse",lat:47.4647,lon:8.5492,size:1},
  {iata:"BRU",icao:"EBBR",name:"Brussels Airport",city:"Bruxelles",country:"Belgique",lat:50.9014,lon:4.4844,size:1},
  {iata:"CPH",icao:"EKCH",name:"Copenhagen Kastrup",city:"Copenhague",country:"Danemark",lat:55.618,lon:12.656,size:1},
  {iata:"ARN",icao:"ESSA",name:"Stockholm Arlanda",city:"Stockholm",country:"Suede",lat:59.6519,lon:17.9186,size:1},
  {iata:"HEL",icao:"EFHK",name:"Helsinki Vantaa",city:"Helsinki",country:"Finlande",lat:60.3172,lon:24.9633,size:1},
  {iata:"OSL",icao:"ENGM",name:"Oslo Gardermoen",city:"Oslo",country:"Norvege",lat:60.1939,lon:11.1004,size:1},
  {iata:"LIS",icao:"LPPT",name:"Lisbon Humberto Delgado",city:"Lisbonne",country:"Portugal",lat:38.7742,lon:-9.1342,size:1},
  {iata:"ATH",icao:"LGAV",name:"Athens Venizelos",city:"Athenes",country:"Grece",lat:37.9364,lon:23.9475,size:1},
  {iata:"DUB",icao:"EIDW",name:"Dublin Airport",city:"Dublin",country:"Irlande",lat:53.4213,lon:-6.2701,size:1},
  {iata:"WAW",icao:"EPWA",name:"Warsaw Chopin",city:"Varsovie",country:"Pologne",lat:52.1657,lon:20.9671,size:1},
  {iata:"PRG",icao:"LKPR",name:"Prague Vaclav Havel",city:"Prague",country:"Tchequie",lat:50.1008,lon:14.26,size:1},
  {iata:"IST",icao:"LTFM",name:"Istanbul Airport",city:"Istanbul",country:"Turquie",lat:41.2753,lon:28.7519,size:1},
  {iata:"SVO",icao:"UUEE",name:"Moscow Sheremetyevo",city:"Moscou",country:"Russie",lat:55.9726,lon:37.4146,size:1},
  {iata:"GVA",icao:"LSGG",name:"Geneva Cointrin",city:"Geneve",country:"Suisse",lat:46.2381,lon:6.1089,size:2},
  {iata:"JFK",icao:"KJFK",name:"John F. Kennedy International",city:"New York",country:"Etats-Unis",lat:40.6413,lon:-73.7781,size:1},
  {iata:"LAX",icao:"KLAX",name:"Los Angeles International",city:"Los Angeles",country:"Etats-Unis",lat:33.9425,lon:-118.4081,size:1},
  {iata:"ORD",icao:"KORD",name:"O'Hare International",city:"Chicago",country:"Etats-Unis",lat:41.9742,lon:-87.9073,size:1},
  {iata:"ATL",icao:"KATL",name:"Hartsfield-Jackson Atlanta",city:"Atlanta",country:"Etats-Unis",lat:33.6407,lon:-84.4277,size:1},
  {iata:"DFW",icao:"KDFW",name:"Dallas/Fort Worth International",city:"Dallas",country:"Etats-Unis",lat:32.8998,lon:-97.0403,size:1},
  {iata:"DEN",icao:"KDEN",name:"Denver International",city:"Denver",country:"Etats-Unis",lat:39.8561,lon:-104.6737,size:1},
  {iata:"SFO",icao:"KSFO",name:"San Francisco International",city:"San Francisco",country:"Etats-Unis",lat:37.6213,lon:-122.379,size:1},
  {iata:"SEA",icao:"KSEA",name:"Seattle-Tacoma International",city:"Seattle",country:"Etats-Unis",lat:47.4502,lon:-122.3088,size:1},
  {iata:"MIA",icao:"KMIA",name:"Miami International",city:"Miami",country:"Etats-Unis",lat:25.7959,lon:-80.287,size:1},
  {iata:"BOS",icao:"KBOS",name:"Boston Logan International",city:"Boston",country:"Etats-Unis",lat:42.3656,lon:-71.0096,size:1},
  {iata:"YYZ",icao:"CYYZ",name:"Toronto Pearson International",city:"Toronto",country:"Canada",lat:43.6772,lon:-79.6306,size:1},
  {iata:"YVR",icao:"CYVR",name:"Vancouver International",city:"Vancouver",country:"Canada",lat:49.1947,lon:-123.1792,size:1},
  {iata:"MEX",icao:"MMMX",name:"Mexico City Benito Juarez",city:"Mexico",country:"Mexique",lat:19.4363,lon:-99.0721,size:1},
  {iata:"DXB",icao:"OMDB",name:"Dubai International",city:"Dubai",country:"EAU",lat:25.2532,lon:55.3657,size:1},
  {iata:"SIN",icao:"WSSS",name:"Singapore Changi",city:"Singapour",country:"Singapour",lat:1.3644,lon:103.9915,size:1},
  {iata:"HKG",icao:"VHHH",name:"Hong Kong International",city:"Hong Kong",country:"Chine",lat:22.308,lon:113.9185,size:1},
  {iata:"NRT",icao:"RJAA",name:"Tokyo Narita",city:"Tokyo",country:"Japon",lat:35.7647,lon:140.3864,size:1},
  {iata:"HND",icao:"RJTT",name:"Tokyo Haneda",city:"Tokyo",country:"Japon",lat:35.5494,lon:139.7798,size:1},
  {iata:"PEK",icao:"ZBAA",name:"Beijing Capital",city:"Pekin",country:"Chine",lat:40.0799,lon:116.6031,size:1},
  {iata:"PVG",icao:"ZSPD",name:"Shanghai Pudong",city:"Shanghai",country:"Chine",lat:31.1443,lon:121.8083,size:1},
  {iata:"SYD",icao:"YSSY",name:"Sydney Kingsford Smith",city:"Sydney",country:"Australie",lat:-33.9399,lon:151.1753,size:1},
  {iata:"GRU",icao:"SBGR",name:"Sao Paulo Guarulhos",city:"Sao Paulo",country:"Bresil",lat:-23.4356,lon:-46.4731,size:1},
  {iata:"JNB",icao:"FAJS",name:"Johannesburg O.R. Tambo",city:"Johannesburg",country:"Afrique du Sud",lat:-26.1392,lon:28.246,size:1},
  {iata:"DOH",icao:"OTHH",name:"Hamad International",city:"Doha",country:"Qatar",lat:25.2731,lon:51.6081,size:1},
  {iata:"ICN",icao:"RKSI",name:"Seoul Incheon",city:"Seoul",country:"Coree du Sud",lat:37.4602,lon:126.4407,size:1},
  {iata:"BKK",icao:"VTBS",name:"Bangkok Suvarnabhumi",city:"Bangkok",country:"Thailande",lat:13.69,lon:100.7501,size:1},
  {iata:"KUL",icao:"WMKK",name:"Kuala Lumpur International",city:"Kuala Lumpur",country:"Malaisie",lat:2.7456,lon:101.7099,size:1},
  {iata:"DEL",icao:"VIDP",name:"Indira Gandhi International",city:"Delhi",country:"Inde",lat:28.5562,lon:77.1,size:1},
  {iata:"BOM",icao:"VABB",name:"Chhatrapati Shivaji Maharaj",city:"Mumbai",country:"Inde",lat:19.0896,lon:72.8656,size:1},
  {iata:"CAI",icao:"HECA",name:"Cairo International",city:"Le Caire",country:"Egypte",lat:30.1219,lon:31.4056,size:1},
  {iata:"AUH",icao:"OMAA",name:"Abu Dhabi International",city:"Abu Dhabi",country:"EAU",lat:24.433,lon:54.6511,size:1},
];

const REGIONS={europe:{minLat:36,maxLat:71,minLon:-12,maxLon:40},france:{minLat:42,maxLat:51.5,minLon:-5,maxLon:10},northamerica:{minLat:24,maxLat:72,minLon:-170,maxLon:-50},world:{minLat:-60,maxLat:80,minLon:-180,maxLon:180}};
const MIL=["RFR","RFF","DUKE","USAF","NATO","ARMY","NAVY","RSAF","FAF","GAF","RNF","VIPER","BISON","FALCON","EAGLE","HAWK","TOMCAT","HORNET","RAPTOR","REAPER","COBRA","APACHE","CHINOOK","MUSTANG","GHOST","SHADOW","TALON"];
const CGO=["UPS","FDX","FED","DHL","ABX","GTI","CLX","BOX","ATN","RCH","NKS"];
const TC={military:{c:"#c44040",mc:"#ff6060",lbl:"Militaire"},commercial:{c:"#cccccc",mc:"#e8e8e8",lbl:"Commercial"},cargo:{c:"#b08828",mc:"#ddaa44",lbl:"Cargo"},private:{c:"#6868c4",mc:"#9090ee",lbl:"Prive"},helicopter:{c:"#3a9a3a",mc:"#55cc55",lbl:"Helicoptere"},unknown:{c:"#484848",mc:"#888888",lbl:"Inconnu"}};
const CTRY={France:"FR",Germany:"DE","United Kingdom":"GB","United States":"US",Russia:"RU",China:"CN",Spain:"ES",Italy:"IT",Netherlands:"NL",Turkey:"TR",Ireland:"IE",Belgium:"BE",Switzerland:"CH",Austria:"AT",Sweden:"SE",Norway:"NO",Denmark:"DK",Poland:"PL",Portugal:"PT",Greece:"GR",Finland:"FI",Canada:"CA",Australia:"AU",Japan:"JP","South Korea":"KR",India:"IN",Brazil:"BR",Mexico:"MX",Israel:"IL","Saudi Arabia":"SA",UAE:"AE",Qatar:"QA",Singapore:"SG"};
const ccode=c=>CTRY[c]||c.slice(0,2).toUpperCase();

function classify(f){
  const cs=(f.callsign||"").toUpperCase().trim(),sq=parseInt(f.squawk||"0",10),alt=f.baro_altitude||0,spd=(f.velocity||0)*3.6;
  for(const p of MIL)if(cs.startsWith(p))return"military";
  if(sq>=6000&&sq<=6777)return"military";
  if(alt<1500&&spd<250&&alt>50)return"helicopter";
  for(const p of CGO)if(cs.startsWith(p))return"cargo";
  if(/^[A-Z]{1,2}\d{3,5}$|^N\d|^G-[A-Z]{4}$|^F-[A-Z]{4}$/.test(cs))return"private";
  if(/^[A-Z]{3}\d{1,4}[A-Z]?$/.test(cs))return"commercial";
  return"unknown";
}

// ===== SHARED STATE =====
let all=[],filt=[],sel=null,hov=null,hovAP=null,selAP=null;
let region="europe",qry="",chip="all";
let trails={},fullT={},meta={};
let refInt=null,src="",aiOpen=false,lastAiCS="",activeTab="route";
let lastAPiata="",apAiLoaded=false,activeAPTab="overview";
let currentView="2d";
let gl=null,currentBM="dark",drawQ=false;
const REGION_PRESETS=JSON.parse(JSON.stringify(REGIONS));

// Globe state
let gViewer=null,globeInited=false;
let gFlightEntities={},gAirportEntities={};
let gTypeFilter="all",gAltMin=0,gAltMax=45000;
let cloudLayer=null,borderLayer=null,osmBuildings=null;
let measuringMode=false,measurePts=[],measureEnts=[];
let gPrevHdg=0,fpsCtr=0,fpsLast=0,fps=60;

// ===== HELPERS =====
const fAlt=m=>m?Math.round(m).toLocaleString("fr-FR"):"N/A";
const fSpd=ms=>ms?Math.round(ms*3.6):"N/A";
const fVR=v=>(v==null)?"N/A":`${v>0?"+":""}${v.toFixed(1)}`;
function fHdg(h){if(!h)return"N/A";const d=["N","NE","E","SE","S","SO","O","NO"];return`${Math.round(h)}deg ${d[Math.round(h/45)%8]}`;}
function ago(ts){if(!ts)return"";const d=Math.floor(Date.now()/1000)-ts;return d<60?`${d}s`:`${Math.floor(d/60)}m`;}
function ll2xy(lat,lon,b,w,h){
  if(gl&&b===REGIONS[region]){const q=gl.project([lon,lat]);return{x:q.x,y:q.y};}
  return{x:((lon-b.minLon)/(b.maxLon-b.minLon))*w,y:((b.maxLat-lat)/(b.maxLat-b.minLat))*h};
}
function fDur(m){if(!m||isNaN(m))return"-";return`${Math.floor(m/60)}h${String(Math.round(m%60)).padStart(2,"0")}`;}
function addM(t,m){if(!t||t==="--:--")return"-";const[h,mn]=t.split(":").map(Number),tot=h*60+mn+m;return`${String(Math.floor(tot/60)%24).padStart(2,"0")}:${String(tot%60).padStart(2,"0")}`;}

// ===== VIEW SWITCHING =====
function switchView(v){
  currentView=v;
  document.getElementById("tab-2d").classList.toggle("on",v==="2d");
  document.getElementById("tab-3d").classList.toggle("on",v==="3d");
  document.getElementById("view-2d").style.display=v==="2d"?"flex":"none";
  document.getElementById("view-3d").style.display=v==="3d"?"flex":"none";
  document.getElementById("cmdbar").style.display=v==="2d"?"flex":"none";
  // Adjust det panel top
  const det=document.getElementById("det");
  if(v==="2d"){det.classList.remove("globe-mode");}
  else{det.classList.add("globe-mode");if(!globeInited)initGlobe();}
  resizeMap();
}

// ===== LEGEND =====
function hideLeg(){document.getElementById("leg").style.display="none";document.getElementById("leg-show").style.display="block";}
function showLeg(){document.getElementById("leg").style.display="block";document.getElementById("leg-show").style.display="none";}

// ===== DIAG =====
let dlogs=[];
function dlog(m,ok){const t=new Date().toLocaleTimeString("fr-FR");dlogs.push(`[${t}] ${ok?"OK":"--"} ${m}`);if(dlogs.length>20)dlogs.shift();const el=document.getElementById("diag-log");if(el)el.innerHTML=dlogs.slice().reverse().join("<br>");}
function togDiag(){const p=document.getElementById("diag-p");p.style.display=p.style.display==="none"?"block":"none";}

// ===== TABS =====
function switchTab(tab){
  activeTab=tab;
  document.querySelectorAll(".tab-btn").forEach(b=>b.classList.toggle("on",b.dataset.tab===tab));
  document.querySelectorAll(".tab-pane").forEach(p=>p.classList.toggle("on",p.id==="tab-"+tab));
  if(tab==="live"&&sel){gauges();drawZoom();}
}
document.getElementById("tab-nav").addEventListener("click",e=>{
  const btn=e.target.closest(".tab-btn");if(!btn)return;switchTab(btn.dataset.tab);
});

// ===== PARSE =====
function parseStates(d){
  if(!d?.states)return[];
  return d.states.filter(s=>s[5]!=null&&s[6]!=null)
    .map(s=>({icao24:s[0],callsign:(s[1]||"").trim()||"UNKNOWN",origin_country:s[2],last_contact:s[4],longitude:s[5],latitude:s[6],baro_altitude:s[7],on_ground:s[8],velocity:s[9],heading:s[10],vertical_rate:s[11],geo_altitude:s[13],squawk:s[14]}))
    .filter(f=>!f.on_ground&&(f.baro_altitude||0)>50)
    .map(f=>({...f,type:classify(f)}));
}

// ===== DEMO =====
function genDemo(b){
  const pool=[{cs:"AFR",cc:"France",t:"commercial"},{cs:"DLH",cc:"Germany",t:"commercial"},{cs:"BAW",cc:"United Kingdom",t:"commercial"},{cs:"EZY",cc:"United Kingdom",t:"commercial"},{cs:"RYR",cc:"Ireland",t:"commercial"},{cs:"KLM",cc:"Netherlands",t:"commercial"},{cs:"UAL",cc:"United States",t:"commercial"},{cs:"FDX",cc:"United States",t:"cargo"},{cs:"UPS",cc:"United States",t:"cargo"},{cs:"DHL",cc:"Germany",t:"cargo"},{cs:"DUKE",cc:"United States",t:"military"},{cs:"RFR",cc:"France",t:"military"},{cs:"VIPER",cc:"United Kingdom",t:"military"},{cs:"N",cc:"United States",t:"private"}];
  return Array.from({length:region==="world"?200:90},(_,i)=>{
    const a=pool[i%pool.length];
    return{icao24:Math.random().toString(16).slice(2,8),callsign:a.cs+String(~~(Math.random()*9000)+1000),origin_country:a.cc,last_contact:Math.floor(Date.now()/1000)-~~(Math.random()*30),longitude:b.minLon+Math.random()*(b.maxLon-b.minLon),latitude:b.minLat+Math.random()*(b.maxLat-b.minLat),baro_altitude:a.t==="military"?500+Math.random()*12000:3000+Math.random()*9000,on_ground:false,velocity:a.t==="military"?120+Math.random()*400:150+Math.random()*280,heading:Math.random()*360,vertical_rate:(Math.random()-.5)*12,geo_altitude:3500+Math.random()*9000,squawk:a.t==="military"?"6101":String(~~(Math.random()*4000)+1000).padStart(4,"0"),type:a.t};
  });
}

// ===== FETCH =====
async function tryFetch(url){
  if(CFG.proxy){try{const r=await fetch(`${CFG.proxy}/flights?region=${region}`,{signal:AbortSignal.timeout(10000)});if(r.ok){const d=await r.json();if(d?.states?.length>0){src="PROXY";return d;}}}catch(e){dlog("Proxy: "+e.message,false);}}
  dlog("Direct OpenSky",null);
  try{const r=await fetch(url,{signal:AbortSignal.timeout(8000)});if(r.ok){const d=await r.json();if(d?.states?.length>0){src="LIVE";dlog("Direct OK",true);return d;}}else dlog("HTTP "+r.status,false);}catch(e){dlog(e.message,false);}
  for(const[n,fn]of[["allorigins",u=>`https://api.allorigins.win/raw?url=${encodeURIComponent(u)}`],["corsproxy",u=>`https://corsproxy.io/?${encodeURIComponent(u)}`]]){
    dlog(n,null);
    try{const r=await fetch(fn(url),{signal:AbortSignal.timeout(12000)});if(r.ok){const d=await r.json();if(d?.states?.length>0){src=n;dlog(n+" OK",true);return d;}}else dlog(n+" "+r.status,false);}catch(e){dlog(n+": "+e.message,false);}
  }
  src="DEMO";return null;
}

async function loadFlights(){
  const b=REGIONS[region];
  const url=`https://opensky-network.org/api/states/all?lamin=${b.minLat}&lamax=${b.maxLat}&lomin=${b.minLon}&lomax=${b.maxLon}`;
  setLed("loading");
  const data=await tryFetch(url);
  if(data){all=parseStates(data);document.getElementById("err-box").style.display="none";}
  else{all=genDemo(b);document.getElementById("err-box").textContent="Mode demonstration - API OpenSky inaccessible. Deployez le proxy Render pour les donnees live.";document.getElementById("err-box").style.display="block";}
  all.forEach(f=>{
    trails[f.icao24]=trails[f.icao24]||[];fullT[f.icao24]=fullT[f.icao24]||[];
    const pt={lat:f.latitude,lon:f.longitude};
    trails[f.icao24].push(pt);fullT[f.icao24].push(pt);
    if(trails[f.icao24].length>14)trails[f.icao24].shift();
  });
  if(src==="DEMO"){all.forEach(f=>{const r=f.heading*Math.PI/180,sp=(f.velocity||200)/111000*15;f.latitude=Math.max(b.minLat+.5,Math.min(b.maxLat-.5,f.latitude+Math.cos(r)*sp));f.longitude=Math.max(b.minLon+.5,Math.min(b.maxLon-.5,f.longitude+Math.sin(r)*sp));});}
  applyFilt();updateStats();
  setLed(src==="DEMO"?"demo":"ok");
  document.getElementById("loader").style.display="none";
  const sl=document.getElementById("src-lbl");if(sl)sl.textContent=src;
  if(sel){const u=all.find(f=>f.icao24===sel.icao24);if(u){sel=u;gauges();drawZoom();progress();}}
  // Update globe if active
  if(globeInited){updateGlobeFlights();document.getElementById("hud-src").textContent=src;document.getElementById("gc-upd").textContent=new Date().toLocaleTimeString("fr-FR");}
  if(selAP)loadAirportFlights(selAP);
}

function setLed(s){
  const led=document.getElementById("led"),txt=document.getElementById("s-txt"),now=new Date().toLocaleTimeString("fr-FR");
  document.getElementById("sv-time").textContent=now;
  if(s==="loading"){led.style.background="#b08828";led.style.animation="none";txt.textContent="Chargement...";}
  else if(s==="ok"){led.style.background="#3a9a3a";led.style.animation="blink 2.5s infinite";txt.textContent="En direct - "+now;}
  else if(s==="demo"){led.style.background="#b08828";led.style.animation="blink 1s infinite";txt.textContent="Demo - "+now;}
  else{led.style.background="#c44040";txt.textContent="Erreur";}
}

// ===== FILTER =====
function applyFilt(){
  const ql=qry.toLowerCase();
  filt=all.filter(f=>{const mq=!ql||f.callsign.toLowerCase().includes(ql)||f.origin_country.toLowerCase().includes(ql)||f.icao24.includes(ql);return mq&&(chip==="all"||f.type===chip);});
  renderList();drawMap();
}
function updateStats(){
  const mil=all.filter(f=>f.type==="military").length,com=all.filter(f=>f.type==="commercial").length,cgo=all.filter(f=>f.type==="cargo").length;
  document.getElementById("sv-tot").textContent=filt.length;
  document.getElementById("sv-mil").textContent=mil;
  document.getElementById("sv-com").textContent=com;
  document.getElementById("sv-cgo").textContent=cgo;
  document.getElementById("sv-oth").textContent=all.length-mil-com-cgo;
  if(globeInited){document.getElementById("gc-tot").textContent=filt.length;document.getElementById("gc-mil").textContent=mil;}
}

// ===== LIST =====
function renderList(){
  document.getElementById("fl-ct")&&(document.getElementById("fl-ct").textContent=filt.length+" vols");
  document.getElementById("sb-aer-ct").textContent=filt.length;
  document.getElementById("fl-list").innerHTML=filt.map(f=>{
    const t=TC[f.type]||TC.unknown,isSel=sel?.icao24===f.icao24;
    return`<div class="fl${isSel?" sel":""}" data-i="${f.icao24}">
      <div class="fl-bar" style="background:${t.c}${isSel?"":"55"}"></div>
      <div class="fl-b"><div class="fl-cs" style="color:${isSel||f.type==="military"?t.c:"var(--t1)"}">${f.callsign} <span style="font-size:9px;color:var(--t4);font-weight:400">${ccode(f.origin_country)}</span></div><div class="fl-m"><span style="color:${t.c}">${t.lbl}</span><span>${f.origin_country}</span></div></div>
      <div class="fl-r"><div class="fl-alt">${Math.round(f.baro_altitude||0)}m</div><div class="fl-ago">${ago(f.last_contact)}</div></div>
    </div>`;
  }).join("");
  document.getElementById("fl-list").querySelectorAll(".fl").forEach(el=>{
    el.addEventListener("click",()=>{const f=all.find(x=>x.icao24===el.dataset.i);if(f)pick(f);});
  });
  if(sbMode==="all")renderAllList();
}

// ===== PICK =====
function pick(f){
  sel=f;selAP=null;renderList();drawMap();showDetail(f);centerOn(f.latitude,f.longitude);
  if(globeInited)updateGlobeFlights();
}

// ===== PANEL =====
function openDet(){document.getElementById("det").classList.add("open");document.getElementById("det-tab").classList.add("hidden");}
function closeDet(){document.getElementById("det").classList.remove("open");document.getElementById("det-tab").classList.remove("hidden");}
function toggleDet(){const det=document.getElementById("det");if(det.classList.contains("open"))closeDet();else openDet();}

function setFlightMode(){
  document.getElementById("det-hd").style.display="block";
  document.getElementById("det-body").style.display="block";
  document.getElementById("det-ap-hd").style.display="none";
  document.getElementById("ap-tab-nav").style.display="none";
  document.getElementById("ap-body").style.display="none";
}
function setAirportMode(){
  document.getElementById("det-hd").style.display="none";
  document.getElementById("det-body").style.display="none";
  document.getElementById("det-ap-hd").style.display="block";
  document.getElementById("ap-tab-nav").style.display="flex";
  document.getElementById("ap-body").style.display="flex";
  document.getElementById("ap-body").style.flexDirection="column";
  document.getElementById("ap-body").style.position="relative";
  document.getElementById("ap-body").style.overflow="hidden";
  document.getElementById("ap-body").style.flex="1";
}
function showPanelFlight(){document.getElementById("det-empty").style.display="none";document.getElementById("det-flight").style.display="block";document.getElementById("tab-nav").style.display="flex";document.getElementById("det-body").style.display="block";setFlightMode();openDet();}
function showPanelEmpty(){document.getElementById("det-empty").style.display="flex";document.getElementById("det-flight").style.display="none";document.getElementById("tab-nav").style.display="none";document.getElementById("det-body").style.display="none";setFlightMode();closeDet();}

// ===== AIRPORT PICK =====
function pickAirport(ap){
  selAP=ap;sel=null;renderList();drawMap();centerOn(ap.lat,ap.lon);
  setAirportMode();openDet();
  document.querySelectorAll(".det-tab-dot").forEach(d=>d.style.background="#4fa3e0");
  document.querySelector(".det-tab-txt").textContent="AEROPORT";
  document.getElementById("ap-iata").textContent=ap.iata;
  document.getElementById("ap-fullname").textContent=ap.name;
  document.getElementById("ap-loc").textContent=`${ap.city}, ${ap.country}  |  ICAO: ${ap.icao}`;
  document.getElementById("ap-pax").textContent="-";
  document.getElementById("ap-rwy").textContent="-";
  const nearby=filt.filter(f=>Math.abs(f.latitude-ap.lat)<.5&&Math.abs(f.longitude-ap.lon)<.5);
  document.getElementById("ap-live-ct").textContent=nearby.length;
  switchAPTab("overview");
  if(lastAPiata!==ap.iata){lastAPiata=ap.iata;apAiLoaded=false;loadAirportAI(ap);}
  loadAirportFlights(ap);
  if(globeInited&&gViewer){gViewer.camera.flyTo({destination:Cesium.Cartesian3.fromDegrees(ap.lon,ap.lat,120000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.75,roll:0},duration:2});}
}

function switchAPTab(tab){
  activeAPTab=tab;
  document.querySelectorAll(".ap-tab-btn").forEach(b=>b.classList.toggle("on",b.dataset.aptab===tab));
  document.querySelectorAll(".ap-pane").forEach(p=>p.classList.toggle("on",p.id==="aptab-"+tab));
}
document.getElementById("ap-tab-nav").addEventListener("click",e=>{const btn=e.target.closest(".ap-tab-btn");if(!btn)return;switchAPTab(btn.dataset.aptab);});
document.getElementById("det-ap-cls").addEventListener("click",()=>{selAP=null;closeDet();setFlightMode();document.getElementById("det-empty").style.display="flex";document.getElementById("det-flight").style.display="none";document.getElementById("tab-nav").style.display="none";drawMap();});

function loadAirportFlights(ap){
  const d=0.5,nearby=filt.filter(f=>Math.abs(f.latitude-ap.lat)<d&&Math.abs(f.longitude-ap.lon)<d);
  const deps=nearby.filter(f=>(f.baro_altitude||0)<2000&&(f.velocity||0)*3.6>100);
  const arrs=nearby.filter(f=>(f.baro_altitude||0)<3000&&(f.vertical_rate||0)<-1);
  function flRow(f,dir){const t=TC[f.type]||TC.unknown;return`<div class="apfl-item" style="cursor:pointer" onclick="pick(all.find(x=>x.icao24==='${f.icao24}'))"><div class="apfl-cs" style="color:${t.c}">${f.callsign}</div><div class="apfl-dest">${f.origin_country}</div><div class="apfl-time">${Math.round(f.baro_altitude||0)}m</div><div class="apfl-st" style="color:${dir==='dep'?'var(--grn)':'var(--amb)'}">${dir==='dep'?'DEP':'ARR'}</div></div>`;}
  document.getElementById("ap-dep-list").innerHTML=deps.length?deps.map(f=>flRow(f,'dep')).join(""):`<div style="font-size:10px;color:var(--t4);padding:4px 0">Aucun depart detecte</div>`;
  document.getElementById("ap-arr-list").innerHTML=arrs.length?arrs.map(f=>flRow(f,'arr')).join(""):`<div style="font-size:10px;color:var(--t4);padding:4px 0">Aucune arrivee detectee</div>`;
  document.getElementById("ap-live-list").innerHTML=nearby.length?nearby.map(f=>{const t=TC[f.type]||TC.unknown;return`<div class="apfl-item" style="cursor:pointer" onclick="pick(all.find(x=>x.icao24==='${f.icao24}'))"><div class="apfl-cs" style="color:${t.c}">${f.callsign}</div><div class="apfl-dest">${t.lbl} - ${f.origin_country}</div><div class="apfl-time">${Math.round(f.baro_altitude||0)}m</div><div class="apfl-time">${fSpd(f.velocity)}</div></div>`;}).join(""):`<div style="font-size:10px;color:var(--t4);padding:8px 0;text-align:center">Aucun trafic ADS-B detecte dans un rayon de 50km</div>`;
}

async function loadAirportAI(ap){
  document.getElementById("ap-ld").style.display="flex";
  ["ap-ident","ap-mgmt","ap-shareholders","ap-runways","ap-staff","ap-airlines","ap-ai-summary","ap-ai-extra"].forEach(id=>{const el=document.getElementById(id);if(el)el.innerHTML="";});
  const prompt=`Expert aéroports. Analyse ${ap.name} (${ap.iata}/${ap.icao}), ${ap.city}, ${ap.country}. Utilise la recherche web. Réponds UNIQUEMENT en JSON valide (sans markdown):
{"annualPassengers":"","annualFlights":"","founded":"","terminals":"","surface":"","director":"","ceo_since":"","staffTotal":"","staffBreakdown":[{"role":"","count":"","pct":10}],"shareholders":[{"name":"","pct":"","type":""}],"runways":[{"name":"","length":"","surface":"","status":""}],"hubAirlines":[],"iataCode":"${ap.iata}","icaoCode":"${ap.icao}","website":"","city":"${ap.city}","country":"${ap.country}","altitude":"","summary":"3-4 phrases"}`;
  try{
    const res=await fetch("https://api.anthropic.com/v1/messages",{method:"POST",headers:aiHeaders(),body:JSON.stringify({model:AI_MODEL,max_tokens:2000,tools:[{type:"web_search_20250305",name:"web_search"}],messages:[{role:"user",content:prompt}]})});
    if(!res.ok)throw new Error("HTTP "+res.status);
    const d=await res.json();
    const txt=d.content.filter(b=>b.type==="text").map(b=>b.text).join("");
    let info={};try{const m=txt.match(/\{[\s\S]*\}/);if(m)info=JSON.parse(m[0]);}catch(e){document.getElementById("ap-ai-summary").textContent=txt;document.getElementById("ap-ld").style.display="none";return;}
    apAiLoaded=true;renderAirportInfo(ap,info);
  }catch(e){document.getElementById("ap-ai-summary").textContent="Erreur IA: "+e.message;document.getElementById("ap-ld").style.display="none";}
}

function renderAirportInfo(ap,info){
  document.getElementById("ap-ld").style.display="none";
  if(info.annualPassengers)document.getElementById("ap-pax").textContent=info.annualPassengers;
  if(info.runways)document.getElementById("ap-rwy").textContent=info.runways.length;
  const kv=(el,rows)=>{document.getElementById(el).innerHTML=rows.map(([k,v])=>`<div class="ap-kv-row"><div>${k}</div><div>${v||"-"}</div></div>`).join("");};
  kv("ap-ident",[["IATA",info.iataCode||ap.iata],["ICAO",info.icaoCode||ap.icao],["Ville",info.city||ap.city],["Pays",info.country||ap.country],["Ouverture",info.founded||"-"],["Altitude",info.altitude||"-"],["Surface",info.surface||"-"],["Terminaux",info.terminals||"-"],["Pax/an",info.annualPassengers||"-"],["Vols/an",info.annualFlights||"-"],["Site web",info.website?`<a href="${info.website}" target="_blank" style="color:#4fa3e0;text-decoration:none">${info.website.replace(/https?:\/\//,"")}</a>`:"-"]]);
  kv("ap-mgmt",[["Directeur général",info.director||"-"],["En poste depuis",info.ceo_since||"-"],["Personnel total",info.staffTotal||"-"]]);
  if(info.shareholders?.length)document.getElementById("ap-shareholders").innerHTML=info.shareholders.map(s=>`<div style="display:flex;align-items:center;gap:7px;margin-bottom:6px"><div style="min-width:38px;text-align:right;font-family:'JetBrains Mono',monospace;font-size:11px;font-weight:700;color:#4fa3e0">${s.pct}</div><div><div style="font-size:10px;color:var(--t1);font-weight:500">${s.name}</div><div style="font-size:8px;color:var(--t4);text-transform:uppercase;letter-spacing:.1em">${s.type||""}</div></div></div>`).join("");
  if(info.runways?.length){const mx=Math.max(...info.runways.map(r=>parseInt(r.length)||0));document.getElementById("ap-runways").innerHTML=info.runways.map(r=>{const l=parseInt(r.length)||0,p=mx>0?(l/mx*100).toFixed(0):50;return`<div class="rwy-item"><div class="rwy-name"><span>${r.name}</span><span style="color:var(--t3);font-size:9px">${r.length} - ${r.surface||""}</span></div><div class="rwy-bar"><div class="rwy-fill" style="width:${p}%"></div></div><div style="font-size:8px;color:var(--t4);margin-top:2px">${r.status||""}</div></div>`;}).join("");}
  if(info.staffBreakdown?.length)document.getElementById("ap-staff").innerHTML=info.staffBreakdown.map(s=>`<div class="staff-item"><div style="min-width:130px;font-size:9px;color:var(--t2)">${s.role}</div><div class="staff-bar-wrap"><div class="staff-bar-fill" style="width:${s.pct||50}%"></div></div><div style="font-size:9px;font-family:'JetBrains Mono',monospace;color:var(--t3);min-width:38px;text-align:right">${s.count}</div></div>`).join("");
  if(info.hubAirlines?.length)document.getElementById("ap-airlines").textContent=info.hubAirlines.join("  •  ");
  if(info.summary)document.getElementById("ap-ai-summary").textContent=info.summary;
  document.getElementById("ap-ai-extra").innerHTML=`<div style="background:var(--bg2);border:1px solid var(--b1);border-radius:3px;padding:9px 11px;margin-bottom:8px"><div style="font-size:8px;text-transform:uppercase;letter-spacing:.14em;color:var(--t4);margin-bottom:5px">Actionnariat</div>${(info.shareholders||[]).map(s=>`<div style="font-size:10px;margin-bottom:3px"><span style="color:#4fa3e0;font-family:'JetBrains Mono',monospace;font-weight:700">${s.pct}</span> - <span style="color:var(--t1)">${s.name}</span> <span style="color:var(--t4)">(${s.type||""})</span></div>`).join("")}</div><div style="background:var(--bg2);border:1px solid var(--b1);border-radius:3px;padding:9px 11px"><div style="font-size:8px;text-transform:uppercase;letter-spacing:.14em;color:var(--t4);margin-bottom:5px">Compagnies hubs</div><div style="font-size:10px;color:var(--t2);line-height:1.8">${(info.hubAirlines||[]).join("  •  ")||"-"}</div></div>`;
}

// ===== SHOW DETAIL =====
function showDetail(f){
  const t=TC[f.type]||TC.unknown;
  showPanelFlight();
  document.querySelectorAll(".det-tab-dot").forEach(d=>d.style.background=t.c);
  document.querySelector(".det-tab-txt").textContent="VOL INFO";
  document.getElementById("det-cc").textContent=ccode(f.origin_country);
  const cs=document.getElementById("det-cs");cs.textContent=f.callsign;cs.style.color=t.c;
  document.getElementById("det-cy").textContent=f.origin_country;
  const b=document.getElementById("det-badge");
  b.textContent=t.lbl.toUpperCase();b.style.color=t.c;b.style.borderColor=t.c+"55";b.style.background=t.c+"11";
  gauges();drawZoom();progress();
  document.getElementById("raw-rows").innerHTML=[
    ["ICAO24",f.icao24],["Callsign",f.callsign],["Pays",f.origin_country],
    ["Alt. baro",fAlt(f.baro_altitude)+" m"],["Alt. geo",fAlt(f.geo_altitude)+" m"],
    ["Vitesse",fSpd(f.velocity)+" km/h"],["Cap",fHdg(f.heading)],
    ["Taux vertical",fVR(f.vertical_rate)+" m/s"],["Squawk",f.squawk||"N/A"],
    ["Latitude",f.latitude?.toFixed(5)],["Longitude",f.longitude?.toFixed(5)],
    ["Dernier contact",ago(f.last_contact)],
  ].map(([k,v])=>`<tr><td>${k}</td><td>${v}</td></tr>`).join("");
  document.getElementById("fr24-a").href=`https://www.flightradar24.com/${f.callsign}`;
}

// ===== GAUGES =====
function gauges(){
  const f=sel;if(!f)return;
  const vr=f.vertical_rate||0;
  document.getElementById("g-alt").textContent=fAlt(f.baro_altitude);
  document.getElementById("g-spd").textContent=fSpd(f.velocity);
  document.getElementById("g-hdg").textContent=f.heading?Math.round(f.heading):"-";
  document.getElementById("g-vr").textContent=fVR(vr);
  const va=document.getElementById("vr-ar");
  va.textContent=vr>0.5?"^":vr<-0.5?"v":"-";
  va.style.color=vr>0.5?"var(--grn)":vr<-0.5?"var(--red)":"var(--t3)";
  document.getElementById("gb-alt").style.width=Math.min(100,((f.baro_altitude||0)/13000)*100)+"%";
  document.getElementById("gb-spd").style.width=Math.min(100,((f.velocity||0)*3.6/1200)*100)+"%";
  document.getElementById("gb-hdg").style.width=((f.heading||0)/360)*100+"%";
  const vb=document.getElementById("gb-vr");
  vb.style.width=Math.min(100,50+((vr||0)/20)*50)+"%";
  vb.className="gc-b "+(vr>0?"g":vr<0?"r":"");
}

// ===== PROGRESS =====
function progress(){
  const f=sel;if(!f)return;
  const m=meta[f.icao24];
  if(!m){document.getElementById("dep-c").textContent="???";document.getElementById("arr-c").textContent="???";document.getElementById("pb-pct").textContent="En attente des donnees IA...";document.getElementById("fnum").textContent=f.callsign;return;}
  document.getElementById("dep-c").textContent=m.depCode||"???";document.getElementById("dep-n").textContent=m.depName||"Depart";document.getElementById("dep-t").textContent=m.depTime||"--:--";
  document.getElementById("arr-c").textContent=m.arrCode||"???";document.getElementById("arr-n").textContent=m.arrName||"Arrivee";document.getElementById("arr-t").textContent=m.arrTime||"--:--";
  document.getElementById("fnum").textContent=m.flightNum||f.callsign;
  const tot=m.totalMin||0,el=m.elapsedMin||0,rem=Math.max(0,tot-el),pct=tot>0?Math.min(100,Math.round((el/tot)*100)):0;
  document.getElementById("pb").style.width=pct+"%";
  document.getElementById("pb-pct").textContent=pct+"% parcouru";
  document.getElementById("t-el").textContent=fDur(el);document.getElementById("t-tot").textContent=fDur(tot);
  document.getElementById("t-rem").textContent=fDur(rem);document.getElementById("t-eta").textContent=m.etaLocal||addM(m.depTime,tot)||"-";
}

// ===== AI =====
function togAI(){
  aiOpen=!aiOpen;
  document.getElementById("ai-bd").style.display=aiOpen?"block":"none";
  document.getElementById("ai-arr").textContent=aiOpen?"Masquer ^":"Lancer v";
  if(aiOpen&&sel&&sel.callsign!==lastAiCS)fetchAI(sel);
}

async function fetchAI(f){
  lastAiCS=f.callsign;
  const ld=document.getElementById("ai-ld"),sm=document.getElementById("ai-sum"),tb=document.getElementById("ai-tbl"),pw=document.getElementById("ph-wr");
  ld.style.display="flex";sm.textContent="";tb.innerHTML="";pw.style.display="none";
  document.getElementById("ai-lt").textContent="Analyse de "+f.callsign+"...";
  const prompt=`Expert analyse de vol. Vol ${f.callsign} (ICAO: ${f.icao24}), pays: ${f.origin_country}, altitude: ${Math.round(f.baro_altitude||0)}m, vitesse: ${Math.round((f.velocity||0)*3.6)}km/h, cap: ${Math.round(f.heading||0)}deg, position: lat=${f.latitude?.toFixed(3)} lon=${f.longitude?.toFixed(3)}. Utilise la recherche web. Reponds UNIQUEMENT en JSON valide sans markdown:
{"flightNum":"","airline":"","registration":"","aircraft":"","seats":0,"depCode":"","depName":"","depTime":"","arrCode":"","arrName":"","arrTime":"","totalMin":0,"elapsedMin":0,"etaLocal":"","alliance":"","yearDelivered":"","passengers":"","summary":"2-3 phrases"}`;
  try{
    const res=await fetch("https://api.anthropic.com/v1/messages",{method:"POST",headers:aiHeaders(),body:JSON.stringify({model:AI_MODEL,max_tokens:1000,tools:[{type:"web_search_20250305",name:"web_search"}],messages:[{role:"user",content:prompt}]})});
    if(!res.ok)throw new Error("API HTTP "+res.status);
    const d=await res.json();
    const txt=d.content.filter(b=>b.type==="text").map(b=>b.text).join("");
    let info={};try{const m=txt.match(/\{[\s\S]*\}/);if(m)info=JSON.parse(m[0]);}catch(e){sm.textContent=txt;ld.style.display="none";return;}
    meta[f.icao24]={flightNum:info.flightNum||f.callsign,depCode:info.depCode||"???",depName:info.depName||"",arrCode:info.arrCode||"???",arrName:info.arrName||"",depTime:info.depTime||"--:--",arrTime:info.arrTime||"--:--",totalMin:+info.totalMin||0,elapsedMin:+info.elapsedMin||0,etaLocal:info.etaLocal||info.arrTime||"-"};
    progress();
    const rai=document.getElementById("route-ai-info");
    if(rai&&info.airline)rai.innerHTML=`<div style="display:grid;grid-template-columns:1fr 1fr;gap:4px;font-size:10px">${[[info.airline,"Compagnie"],[info.registration,"Immatriculation"],[info.aircraft,"Appareil"],[info.passengers,"Passagers"]].filter(([v])=>v).map(([v,k])=>`<div style="background:var(--bg2);border:1px solid var(--b1);border-radius:2px;padding:4px 6px"><div style="font-size:8px;color:var(--t3);text-transform:uppercase;letter-spacing:.1em">${k}</div><div style="font-family:'JetBrains Mono',monospace;color:var(--t1);font-size:10px;margin-top:2px">${v}</div></div>`).join("")}</div>`;
    const rows=[["N de vol",info.flightNum],["Compagnie",info.airline],["Immatriculation",info.registration],["Appareil",info.aircraft],["Sieges",info.seats],["Passagers",info.passengers],["Alliance",info.alliance],["Livraison",info.yearDelivered]].filter(([,v])=>v);
    tb.innerHTML=rows.map(([k,v])=>`<tr><td>${k}</td><td>${v}</td></tr>`).join("");
    if(info.summary)sm.textContent=info.summary;
    if(info.registration)fetchPhotos(info.registration);
    ld.style.display="none";
  }catch(e){ld.style.display="none";sm.textContent="Erreur IA: "+e.message;}
}

async function fetchPhotos(reg){
  const pw=document.getElementById("ph-wr"),pg=document.getElementById("ph-gr");
  try{const r=await fetch(`https://api.planespotters.net/pub/photos/reg/${reg}`,{signal:AbortSignal.timeout(6000)});if(r.ok){const d=await r.json();if(d.photos?.length>0){pg.innerHTML=d.photos.slice(0,4).map(p=>`<img class="ph-img" src="${p.thumbnail_large?.src||p.thumbnail?.src||""}" alt="${reg}" onclick="openLB('${p.link||p.thumbnail_large?.src||""}')" onerror="this.parentElement.removeChild(this)">`).join("");pw.style.display="block";return;}}}catch(e){}
  pg.innerHTML=`<div style="grid-column:span 2;font-size:10px;color:var(--t4);padding:6px 0">Aucune photo pour ${reg} - <a href="https://www.planespotters.net/search?q=${encodeURIComponent(reg||"")}" target="_blank" style="color:#8080cc">Planespotters</a></div>`;pw.style.display="block";
}
function openLB(url){if(!url)return;document.getElementById("lb-img").src=url;document.getElementById("lb").classList.add("on");}

// ===== 2D ZOOM CANVAS =====
function drawZoom(){
  const f=sel;if(!f)return;
  const zcan=document.getElementById("zc"),zw=zcan.clientWidth||294;
  zcan.width=zw;zcan.height=108;
  const zx=zcan.getContext("2d"),full=fullT[f.icao24]||[],t=TC[f.type]||TC.unknown;
  const mg=.6,pts=[...full,{lat:f.latitude,lon:f.longitude}];
  let mnLat=f.latitude-mg,mxLat=f.latitude+mg,mnLon=f.longitude-mg*1.5,mxLon=f.longitude+mg*1.5;
  if(full.length>2){mnLat=Math.min(...pts.map(p=>p.lat))-.2;mxLat=Math.max(...pts.map(p=>p.lat))+.2;mnLon=Math.min(...pts.map(p=>p.lon))-.2;mxLon=Math.max(...pts.map(p=>p.lon))+.2;}
  const zb={minLat:mnLat,maxLat:mxLat,minLon:mnLon,maxLon:mxLon};
  zx.fillStyle="#060606";zx.fillRect(0,0,zw,108);
  zx.strokeStyle="rgba(255,255,255,.025)";zx.lineWidth=1;
  for(let i=0;i<zw;i+=28){zx.beginPath();zx.moveTo(i,0);zx.lineTo(i,108);zx.stroke();}
  for(let j=0;j<108;j+=28){zx.beginPath();zx.moveTo(0,j);zx.lineTo(zw,j);zx.stroke();}
  if(full.length>1){zx.beginPath();full.forEach((p,i)=>{const{x,y}=ll2xy(p.lat,p.lon,zb,zw,108);i===0?zx.moveTo(x,y):zx.lineTo(x,y);});zx.strokeStyle="rgba(190,190,190,.3)";zx.lineWidth=1.5;zx.setLineDash([]);zx.stroke();}
  const cx=zw/2,cy=54,rd=((f.heading||0)*Math.PI)/180,sz=8;
  zx.save();zx.translate(cx,cy);zx.rotate(rd);
  zx.fillStyle="#ffffff";zx.shadowColor=t.mc;zx.shadowBlur=10;
  zx.beginPath();zx.moveTo(0,-sz);zx.lineTo(sz*.62,sz*.75);zx.lineTo(0,sz*.28);zx.lineTo(-sz*.62,sz*.75);zx.closePath();zx.fill();zx.restore();
  zx.setLineDash([4,4]);zx.beginPath();zx.moveTo(cx,cy);zx.lineTo(cx+Math.sin(rd)*30,cy-Math.cos(rd)*30);zx.strokeStyle=t.mc+"66";zx.lineWidth=1;zx.stroke();zx.setLineDash([]);
}

// ===== 2D DRAWMAP =====
const cvs=document.getElementById("mc");
function resizeMap(){
  const mw=document.getElementById("mw");
  if(currentView!=="2d")return;
  cvs.width=mw.clientWidth;cvs.height=mw.clientHeight;
  if(gl)gl.resize();
  drawMap();
}
window.addEventListener("resize",resizeMap);

function drawMap(){
  if(currentView!=="2d")return;
  const b=REGIONS[region],w=cvs.width,h=cvs.height;
  if(!w||!h)return;
  const ctx=cvs.getContext("2d");
  if(gl){ctx.clearRect(0,0,w,h);}
  else{
    ctx.fillStyle="#060606";ctx.fillRect(0,0,w,h);
    ctx.strokeStyle="rgba(255,255,255,.028)";ctx.lineWidth=1;
    for(let i=0;i<w;i+=60){ctx.beginPath();ctx.moveTo(i,0);ctx.lineTo(i,h);ctx.stroke();}
    for(let j=0;j<h;j+=60){ctx.beginPath();ctx.moveTo(0,j);ctx.lineTo(w,j);ctx.stroke();}
  }
  // Airports
  const visAP=AIRPORTS.filter(ap=>{const ms=region==="world"?1:2;return ap.lat>=b.minLat&&ap.lat<=b.maxLat&&ap.lon>=b.minLon&&ap.lon<=b.maxLon&&ap.size<=ms;});
  visAP.forEach(ap=>{
    const{x,y}=ll2xy(ap.lat,ap.lon,b,w,h);
    const isSel=selAP?.iata===ap.iata,isHov=hovAP?.iata===ap.iata,sz=isSel||isHov?7:5;
    ctx.save();ctx.translate(x,y);
    ctx.fillStyle=isSel?"#4fa3e0":isHov?"#7dc4f0":"rgba(79,163,224,.55)";
    ctx.strokeStyle=isSel?"#7dc4f0":"rgba(79,163,224,.7)";ctx.lineWidth=1;
    ctx.shadowColor="#4fa3e0";ctx.shadowBlur=isSel?12:isHov?6:3;
    ctx.beginPath();ctx.rect(-sz,-sz,sz*2,sz*2);ctx.fill();ctx.stroke();
    ctx.shadowBlur=0;ctx.strokeStyle=isSel?"rgba(0,0,0,.6)":"rgba(0,0,0,.4)";ctx.lineWidth=1.2;
    ctx.beginPath();ctx.moveTo(-sz*.6,0);ctx.lineTo(sz*.6,0);ctx.moveTo(0,-sz*.6);ctx.lineTo(0,sz*.6);ctx.stroke();
    ctx.restore();
    if(isSel||isHov||(ap.size===1&&region!=="world")||(region==="france")){
      ctx.font=`${isSel||isHov?"bold ":""}9px 'JetBrains Mono',monospace`;
      ctx.fillStyle=isSel?"#7dc4f0":"rgba(79,163,224,.7)";
      ctx.shadowColor="#4fa3e0";ctx.shadowBlur=isSel?4:2;
      ctx.fillText(ap.iata,x+sz+3,y+4);ctx.shadowBlur=0;
    }
  });
  filt.forEach(f=>{
    const{x,y}=ll2xy(f.latitude,f.longitude,b,w,h);
    if(x<-20||x>w+20||y<-20||y>h+20)return;
    const t=TC[f.type]||TC.unknown,isSel=sel?.icao24===f.icao24,isHov=hov?.icao24===f.icao24;
    const tr=trails[f.icao24]||[];
    if(tr.length>1){ctx.beginPath();tr.forEach((p,i)=>{const tp=ll2xy(p.lat,p.lon,b,w,h);i===0?ctx.moveTo(tp.x,tp.y):ctx.lineTo(tp.x,tp.y);});ctx.strokeStyle="rgba(160,160,160,.17)";ctx.lineWidth=1;ctx.setLineDash([]);ctx.stroke();}
    if(isSel){
      const ft=fullT[f.icao24]||[];
      if(ft.length>1){ctx.beginPath();ft.forEach((p,i)=>{const tp=ll2xy(p.lat,p.lon,b,w,h);i===0?ctx.moveTo(tp.x,tp.y):ctx.lineTo(tp.x,tp.y);});ctx.strokeStyle="#3a9a3a";ctx.lineWidth=2;ctx.shadowColor="#3a9a3a";ctx.shadowBlur=5;ctx.setLineDash([]);ctx.stroke();ctx.shadowBlur=0;}
      ctx.beginPath();ctx.arc(x,y,13,0,Math.PI*2);ctx.strokeStyle=t.c+"44";ctx.lineWidth=1.5;ctx.shadowColor=t.c;ctx.shadowBlur=8;ctx.stroke();ctx.shadowBlur=0;
      ctx.beginPath();ctx.arc(x,y,21,0,Math.PI*2);ctx.strokeStyle=t.c+"14";ctx.lineWidth=1;ctx.stroke();
    }
    const sz=isSel?8:isHov?7:5;
    ctx.save();ctx.translate(x,y);ctx.rotate(((f.heading||0)*Math.PI)/180);
    ctx.beginPath();ctx.moveTo(0,-sz);ctx.lineTo(sz*.62,sz*.75);ctx.lineTo(0,sz*.28);ctx.lineTo(-sz*.62,sz*.75);ctx.closePath();
    ctx.fillStyle=isSel?"#ffffff":t.mc;ctx.shadowColor=t.mc;ctx.shadowBlur=isSel?10:3;ctx.fill();ctx.restore();
    if(f.type==="military"&&(isSel||isHov)){ctx.font="bold 9px 'JetBrains Mono',monospace";ctx.fillStyle=t.c;ctx.fillText("MIL",x+8,y-10);}
    if(isSel||isHov){ctx.font="500 10px 'Inter',sans-serif";ctx.fillStyle=t.mc;ctx.shadowColor=t.mc;ctx.shadowBlur=4;ctx.fillText(f.callsign,x+10,y+(f.type==="military"?0:-9));ctx.shadowBlur=0;}
  });
  // Maritime layers
  if(showZones) drawStrategicZones(ctx,b,w,h);
  if(showPorts) drawPortsOnMap(ctx,b,w,h);
  if(showShips) drawShipsOnMap(ctx,b,w,h);

  document.getElementById("minfo").textContent=`${b.minLat}deg-${b.maxLat}deg N  |  ${b.minLon}deg-${b.maxLon}deg\n${filt.length} vols  |  ${vessels.length} navires  |  ${filt.filter(f=>f.type==="military").length} mil. aer.${src==="DEMO"?"  [AVIONS SIMULES]":""}${aisConnected?"":"  [NAVIRES SIMULES]"}`;
}

// ===== MAP EVENTS =====
cvs.addEventListener("mousemove",e=>{
  const rc=cvs.getBoundingClientRect(),mx=e.clientX-rc.left,my=e.clientY-rc.top;
  const b=REGIONS[region],w=cvs.width,h=cvs.height;
  let cl=null,md=16;
  filt.forEach(f=>{const{x,y}=ll2xy(f.latitude,f.longitude,b,w,h);const d=Math.hypot(x-mx,y-my);if(d<md){md=d;cl=f;}});
  let ca=null,mad=14;
  const visAP=AIRPORTS.filter(ap=>{const ms=region==="world"?1:2;return ap.lat>=b.minLat&&ap.lat<=b.maxLat&&ap.lon>=b.minLon&&ap.lon<=b.maxLon&&ap.size<=ms;});
  visAP.forEach(ap=>{const{x,y}=ll2xy(ap.lat,ap.lon,b,w,h);const d=Math.hypot(x-mx,y-my);if(d<mad){mad=d;ca=ap;}});
  if(md<10)ca=null;
  let changed=false;
  if(cl!==hov){hov=cl;changed=true;}
  if(ca!==hovAP){hovAP=ca;changed=true;}
  if(changed){cvs.style.cursor=(cl||ca)?"pointer":"crosshair";drawMap();}
  const tip=document.getElementById("htip");
  if(ca&&!selAP&&!cl){tip.style.display="block";const nearby=filt.filter(f=>Math.abs(f.latitude-ca.lat)<.4&&Math.abs(f.longitude-ca.lon)<.4).length;tip.innerHTML=`<strong style="color:#4fa3e0">${ca.iata}</strong>  <span style="color:var(--t3);font-size:10px">${ca.icao}</span><br><span style="color:var(--t1);font-weight:600">${ca.name}</span><br><span style="color:var(--t3)">${ca.city}, ${ca.country}  |  ${nearby} appareils proches</span>`;}
  else if(cl&&!sel){const t=TC[cl.type]||TC.unknown;tip.style.display="block";tip.innerHTML=`<strong style="color:${t.c}">${cl.callsign}</strong>  <span style="color:var(--t3);font-size:10px">${ccode(cl.origin_country)} - ${t.lbl}</span><br><span style="color:var(--t3)">${cl.origin_country}  |  ${fAlt(cl.baro_altitude)} m  |  ${fSpd(cl.velocity)} km/h  |  Cap ${Math.round(cl.heading||0)}deg</span>`;}
  else tip.style.display="none";
});
cvs.addEventListener("mouseleave",()=>{hov=null;hovAP=null;document.getElementById("htip").style.display="none";cvs.style.cursor="crosshair";drawMap();});
cvs.addEventListener("click",ev=>{
  cvs.dispatchEvent(new MouseEvent("mousemove",{clientX:ev.clientX,clientY:ev.clientY}));
  // Priority: flight > airport > ship > port
  if(hov)pick(hov);
  else if(hovAP)pickAirport(hovAP);
  else if(hovShip)pickShip(hovShip);
  else if(hovPort)pickPort(hovPort);
});

// ===== REGION / CHIPS =====
document.getElementById("regions").addEventListener("click",e=>{
  const btn=e.target.closest(".rb");if(!btn)return;
  document.querySelectorAll("#regions .rb").forEach(b=>b.classList.remove("on"));btn.classList.add("on");region=btn.dataset.r;fitRegion();
  sel=null;selAP=null;selShip=null;selPort=null;trails={};fullT={};meta={};shipTrails={};
  showPanelEmpty();
  document.getElementById("loader").style.display="flex";
  all=[];filt=[];vessels=[];renderList();
  clearInterval(refInt);clearInterval(vesselInt);
  loadFlights();refInt=setInterval(loadFlights,15000);
  loadVessels();vesselInt=setInterval(loadVessels,30000);
});
document.querySelectorAll(".chip").forEach(c=>{c.addEventListener("click",()=>{document.querySelectorAll(".chip").forEach(x=>x.classList.remove("on"));c.classList.add("on");chip=c.dataset.f;applyFilt();updateStats();});});
document.getElementById("s-input").addEventListener("input",e=>{qry=e.target.value;applyFilt();updateStats();});
document.getElementById("ref-btn").addEventListener("click",()=>{loadFlights();loadVessels();});
document.getElementById("det-cls").addEventListener("click",()=>{sel=null;showPanelEmpty();drawMap();if(globeInited)updateGlobeFlights();});

// ===== TICK =====
setInterval(()=>{
  if(sel){
    if(activeTab==="live"){gauges();drawZoom();}else{gauges();}
    const m=meta[sel?.icao24];if(m&&m.elapsedMin<m.totalMin){m.elapsedMin+=1/60;progress();}
  }
  if(selAP&&activeAPTab==="flights")loadAirportFlights(selAP);
},1000);

// ============================================================
// ===== GLOBE 3D FUNCTIONS =====
// ============================================================

function makePlaneCanvas(color,size=26){
  const c=document.createElement("canvas");c.width=c.height=size;
  const x=c.getContext("2d"),h=size/2,sz=size*.36;
  x.fillStyle=color;x.shadowColor=color;x.shadowBlur=6;
  x.beginPath();x.moveTo(h,2);x.lineTo(h+sz,h+sz);x.lineTo(h,h*.8);x.lineTo(h-sz,h+sz);x.closePath();x.fill();
  return c;
}

function makeAirportCanvas(){
  const c=document.createElement("canvas");c.width=c.height=18;
  const x=c.getContext("2d"),h=9,s=5;
  x.fillStyle="rgba(79,163,224,.4)";x.strokeStyle="#4fa3e0";x.lineWidth=1.5;
  x.fillRect(h-s,h-s,s*2,s*2);x.strokeRect(h-s,h-s,s*2,s*2);
  x.strokeStyle="rgba(79,163,224,.8)";x.lineWidth=1;
  x.beginPath();x.moveTo(h-s*.7,h);x.lineTo(h+s*.7,h);x.moveTo(h,h-s*.7);x.lineTo(h,h+s*.7);x.stroke();
  return c;
}

function initGlobe(){
  if(globeInited)return;
  globeInited=true;
  const loader=document.getElementById("globe-loader");
  Cesium.Ion.defaultAccessToken=CFG.cesium||"";
  gViewer=new Cesium.Viewer("cesiumContainer",{
    terrainProvider:new Cesium.EllipsoidTerrainProvider(),
    baseLayerPicker:false,geocoder:false,homeButton:false,
    infoBox:false,sceneModePicker:false,selectionIndicator:false,
    navigationHelpButton:false,animation:false,timeline:false,
    fullscreenButton:false,vrButton:false,
    creditContainer:document.createElement("div"),
  });
  gViewer.imageryLayers.removeAll();
  gViewer.imageryLayers.addImageryProvider(new Cesium.ArcGisMapServerImageryProvider({url:"https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer"}));
  gViewer.scene.globe.enableLighting=false;
  gViewer.scene.globe.showGroundAtmosphere=true;
  gViewer.scene.skyAtmosphere.show=true;
  gViewer.scene.fog.enabled=true;
  gViewer.scene.globe.depthTestAgainstTerrain=false;
  gViewer.camera.setView({destination:Cesium.Cartesian3.fromDegrees(10,46,8000000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.9,roll:0}});
  setupGlobeMouseHandlers();
  setupGlobeCameraListener();
  setTimeout(()=>{
    loader.style.transition="opacity .4s";loader.style.opacity="0";
    setTimeout(()=>loader.style.display="none",400);
    addGlobeAirports();
    updateGlobeFlights();
    addGlobePorts();
    updateGlobeShips();
    addGlobeStrategicZones();
    globeNotif("Globe 3D initialisé","#4fa3e0");
    document.getElementById("gc-ap").textContent=AIRPORTS.length;
    document.getElementById("gc-ports").textContent=PORTS.length;
  },1200);
}

function globeNotif(msg,color="#eeeeee"){
  const n=document.getElementById("globe-notif");
  n.textContent=msg;n.style.color=color||"#eeeeee";n.style.display="block";
  clearTimeout(n._t);n._t=setTimeout(()=>n.style.display="none",2800);
}

// Globe: update flight entities
function updateGlobeFlights(){
  if(!gViewer)return;
  const gFilt=filt.filter(f=>{
    if(gTypeFilter!=="all"&&f.type!==gTypeFilter)return false;
    const altFt=(f.baro_altitude||0)*3.28084;
    return altFt>=gAltMin&&altFt<=gAltMax;
  });
  const existing=new Set(Object.keys(gFlightEntities));
  gFilt.forEach(f=>{
    const pos=Cesium.Cartesian3.fromDegrees(f.longitude,f.latitude,f.baro_altitude||0);
    const t=TC[f.type]||TC.unknown,isSel=sel?.icao24===f.icao24;
    if(gFlightEntities[f.icao24]){
      const e=gFlightEntities[f.icao24];
      e.position=new Cesium.ConstantPositionProperty(pos);
      e.billboard.rotation=-Cesium.Math.toRadians(f.heading||0);
      e.billboard.scale=isSel?1.6:1;e.label.show=isSel;
      e._fd=f;existing.delete(f.icao24);
    } else {
      const e=gViewer.entities.add({
        id:"gf_"+f.icao24,position:pos,
        billboard:{image:makePlaneCanvas(t.mc,isSel?32:24),scale:isSel?1.6:1,rotation:-Cesium.Math.toRadians(f.heading||0),alignedAxis:Cesium.Cartesian3.UNIT_Z,verticalOrigin:Cesium.VerticalOrigin.CENTER,disableDepthTestDistance:Number.POSITIVE_INFINITY},
        label:{text:f.callsign,font:"bold 9px JetBrains Mono,monospace",fillColor:Cesium.Color.fromCssColorString(t.mc),outlineColor:Cesium.Color.BLACK,outlineWidth:2,style:Cesium.LabelStyle.FILL_AND_OUTLINE,pixelOffset:new Cesium.Cartesian2(14,0),disableDepthTestDistance:Number.POSITIVE_INFINITY,show:isSel},
        show:document.getElementById("tog-flights").checked,
      });
      e._fd=f;gFlightEntities[f.icao24]=e;
    }
  });
  existing.forEach(id=>{if(gFlightEntities[id]){gViewer.entities.remove(gFlightEntities[id]);delete gFlightEntities[id];}});
  document.getElementById("gc-tot").textContent=gFilt.length;
  document.getElementById("gc-mil").textContent=gFilt.filter(f=>f.type==="military").length;
}

function addGlobeAirports(){
  AIRPORTS.forEach(ap=>{
    const pos=Cesium.Cartesian3.fromDegrees(ap.lon,ap.lat,0);
    const e=gViewer.entities.add({
      id:"ga_"+ap.iata,position:pos,
      billboard:{image:makeAirportCanvas(),scale:1,verticalOrigin:Cesium.VerticalOrigin.CENTER,disableDepthTestDistance:Number.POSITIVE_INFINITY,scaleByDistance:new Cesium.NearFarScalar(200000,1.2,10000000,0.3)},
      label:{text:ap.iata,font:"bold 9px JetBrains Mono,monospace",fillColor:Cesium.Color.fromCssColorString("#4fa3e0"),outlineColor:Cesium.Color.BLACK,outlineWidth:2,style:Cesium.LabelStyle.FILL_AND_OUTLINE,pixelOffset:new Cesium.Cartesian2(12,0),disableDepthTestDistance:Number.POSITIVE_INFINITY,scaleByDistance:new Cesium.NearFarScalar(200000,1,8000000,0),translucencyByDistance:new Cesium.NearFarScalar(500000,1,8000000,0)},
      show:document.getElementById("tog-airports").checked,
    });
    e._apd=ap;gAirportEntities[ap.iata]=e;
  });
}

// Globe mouse events
function setupGlobeMouseHandlers(){
  const handler=new Cesium.ScreenSpaceEventHandler(gViewer.scene.canvas);
  handler.setInputAction(ev=>{
    const picked=gViewer.scene.pick(ev.position);
    if(!picked?.id)return;
    const eid=typeof picked.id==="string"?picked.id:picked.id?.id;
    if(typeof eid==="string"&&eid.startsWith("gf_")){const icao=eid.replace("gf_","");const f=filt.find(x=>x.icao24===icao)||gFlightEntities[icao]?._fd;if(f)pick(f);}
    else if(typeof eid==="string"&&eid.startsWith("ga_")){const iata=eid.replace("ga_","");const ap=AIRPORTS.find(a=>a.iata===iata);if(ap)pickAirport(ap);}
    else if(typeof eid==="string"&&eid.startsWith("gs_")){const mmsi=eid.replace("gs_","");const v=vessels.find(x=>x.mmsi===mmsi)||gShipEntities[mmsi]?._vd;if(v)pickShip(v);}
    else if(typeof eid==="string"&&eid.startsWith("gp_")){const pid=eid.replace("gp_","");const p=PORTS.find(x=>x.id===pid);if(p)pickPort(p);}
    if(measuringMode){const pos=getGlobePos(ev.position);if(pos)addMeasurePoint(pos);}
  },Cesium.ScreenSpaceEventType.LEFT_CLICK);
  handler.setInputAction(ev=>{
    const picked=gViewer.scene.pick(ev.position);const eid=typeof picked?.id==="string"?picked.id:picked?.id?.id;
    gViewer.scene.canvas.style.cursor=(eid?.startsWith("gf_")||eid?.startsWith("ga_"))?"pointer":measuringMode?"crosshair":"default";
  },Cesium.ScreenSpaceEventType.MOUSE_MOVE);
  handler.setInputAction(ev=>{
    const pos=getGlobePos(ev.position);
    if(pos&&!measuringMode)gViewer.camera.flyTo({destination:Cesium.Cartesian3.fromDegrees(pos.longitude,pos.latitude,80000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.8,roll:0},duration:1.8});
  },Cesium.ScreenSpaceEventType.LEFT_DOUBLE_CLICK);
}

function getGlobePos(wp){
  try{const r=gViewer.camera.getPickRay(wp);const c=gViewer.scene.globe.pick(r,gViewer.scene);if(c){const cd=Cesium.Cartographic.fromCartesian(c);return{longitude:Cesium.Math.toDegrees(cd.longitude),latitude:Cesium.Math.toDegrees(cd.latitude)};}}catch(e){}return null;
}

function setupGlobeCameraListener(){
  let fps2=60,ft2=0,ff=0;
  gViewer.scene.postRender.addEventListener(()=>{
    const cam=gViewer.camera;
    const cd=Cesium.Cartographic.fromCartesian(cam.position);
    const lat=Cesium.Math.toDegrees(cd.latitude).toFixed(4),lon=Cesium.Math.toDegrees(cd.longitude).toFixed(4),alt=Math.round(cd.height/1000);
    document.getElementById("hud-lat").textContent=lat+"°";
    document.getElementById("hud-lon").textContent=lon+"°";
    document.getElementById("hud-cam-alt").textContent=(alt>1000?(alt/1000).toFixed(1)+"k":alt)+" km";
    const hd=Cesium.Math.toDegrees(cam.heading);
    if(Math.abs(hd-gPrevHdg)>0.5){document.getElementById("compass-ring").style.transform=`rotate(${-hd}deg)`;gPrevHdg=hd;}
    const w=gViewer.scene.canvas.width,ht=gViewer.scene.canvas.height;
    const p1=gViewer.scene.camera.pickEllipsoid(new Cesium.Cartesian2(w/2-50,ht/2));
    const p2=gViewer.scene.camera.pickEllipsoid(new Cesium.Cartesian2(w/2+50,ht/2));
    if(p1&&p2){const d=Cesium.Cartesian3.distance(p1,p2);document.getElementById("hud-scale-lbl").textContent=d>1000?(d/1000).toFixed(0)+" km":Math.round(d)+" m";}
    ff++;const now=performance.now();
    if(now-ft2>1000){fps2=Math.round(ff*1000/(now-ft2));document.getElementById("hud-fps").textContent=fps2;ff=0;ft2=now;}
    if(sel){const upd=filt.find(f=>f.icao24===sel.icao24);if(upd){sel=upd;gauges();}}
  });
}

// Globe layers
function switchImagery(type,btn){
  document.querySelectorAll(".img-btn").forEach(b=>b.classList.remove("on"));if(btn)btn.classList.add("on");
  if(!gViewer)return;
  const layers=gViewer.imageryLayers;while(layers.length>0)layers.remove(layers.get(0));
  const providers={
    satellite:()=>new Cesium.ArcGisMapServerImageryProvider({url:"https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer"}),
    streets:()=>new Cesium.UrlTemplateImageryProvider({url:"https://tile.openstreetmap.org/{z}/{x}/{y}.png",maximumLevel:19,credit:"OpenStreetMap"}),
    hybrid:()=>new Cesium.ArcGisMapServerImageryProvider({url:"https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer"}),
    topo:()=>new Cesium.ArcGisMapServerImageryProvider({url:"https://services.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer"}),
    night:()=>new Cesium.WebMapServiceImageryProvider({url:"https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi",layers:"VIIRS_Black_Marble",parameters:{transparent:true,format:"image/png"},tileWidth:512,tileHeight:512,tilingScheme:new Cesium.GeographicTilingScheme()}),
    dark:()=>new Cesium.UrlTemplateImageryProvider({url:"https://tile.openstreetmap.org/{z}/{x}/{y}.png",maximumLevel:19,credit:"OSM"}),
  };
  if(providers[type])layers.addImageryProvider(providers[type]());
  if(type==="hybrid"){const l=layers.addImageryProvider(new Cesium.UrlTemplateImageryProvider({url:"https://tile.openstreetmap.org/{z}/{x}/{y}.png",maximumLevel:18,credit:"OSM"}));if(l)l.alpha=0.35;}
  if(type==="night"){gViewer.scene.globe.showGroundAtmosphere=false;gViewer.scene.skyAtmosphere.show=false;}
  else{gViewer.scene.globe.showGroundAtmosphere=true;}
  if(cloudLayer)layers.add(cloudLayer);if(borderLayer)layers.add(borderLayer);
}

function toggleGlobeFlights(v){Object.values(gFlightEntities).forEach(e=>{if(e&&!e.isDestroyed)e.show=v;});}
function toggleGlobeAirports(v){Object.values(gAirportEntities).forEach(e=>{if(e&&!e.isDestroyed)e.show=v;});}
function toggleAtmos(v){if(!gViewer)return;gViewer.scene.skyAtmosphere.show=v;gViewer.scene.globe.showGroundAtmosphere=v;gViewer.scene.fog.enabled=v;}
function toggleSunLight(v){if(!gViewer)return;gViewer.scene.globe.enableLighting=v;}
async function toggleBuildings(v){
  if(!gViewer)return;
  if(v){const tok=document.getElementById("ion-token").value.trim();if(!tok){globeNotif("Token Cesium Ion requis pour les bâtiments 3D","#b08828");document.getElementById("tog-buildings").checked=false;return;}
  try{Cesium.Ion.defaultAccessToken=tok;osmBuildings=await Cesium.createOsmBuildingsAsync();gViewer.scene.primitives.add(osmBuildings);globeNotif("Bâtiments 3D OSM activés","#4fa3e0");}catch(e){globeNotif("Erreur bâtiments 3D","#c44040");document.getElementById("tog-buildings").checked=false;}}
  else if(osmBuildings){gViewer.scene.primitives.remove(osmBuildings);osmBuildings=null;}
}
function toggleBorders(v){
  if(!gViewer)return;
  if(v&&!borderLayer){borderLayer=gViewer.imageryLayers.addImageryProvider(new Cesium.UrlTemplateImageryProvider({url:"https://services.arcgisonline.com/ArcGIS/rest/services/Reference/World_Boundaries_and_Places/MapServer/tile/{z}/{y}/{x}",credit:"ESRI"}));borderLayer.alpha=0.7;}
  else if(borderLayer)borderLayer.show=v;
}
function toggleClouds(v){
  if(!gViewer)return;
  if(v&&!cloudLayer){cloudLayer=gViewer.imageryLayers.addImageryProvider(new Cesium.WebMapServiceImageryProvider({url:"https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi",layers:"MODIS_Terra_CorrectedReflectance_TrueColor",parameters:{transparent:true,format:"image/png"},tilingScheme:new Cesium.GeographicTilingScheme()}));cloudLayer.alpha=0.32;}
  else if(cloudLayer)cloudLayer.show=v;
}
function applyIonToken(){const tok=document.getElementById("ion-token").value.trim();CFG.cesium=tok;saveCfgLS();if(!tok){globeNotif("Token vide","#c44040");return;}if(gViewer)Cesium.Ion.defaultAccessToken=tok;document.getElementById("ion-status").textContent="Token appliqué";globeNotif("Token Cesium Ion configuré","#3a9a3a");}
function setGlobeTypeFilter(t,btn){document.querySelectorAll("#gctrl .fchip").forEach(b=>b.classList.remove("on"));if(btn)btn.classList.add("on");gTypeFilter=t;updateGlobeFlights();}
function updateGlobeAltFilter(){const mn=parseInt(document.getElementById("galt-min").value),mx=parseInt(document.getElementById("galt-max").value);document.getElementById("galt-min-lbl").textContent=mn.toLocaleString("fr-FR")+" ft";document.getElementById("galt-max-lbl").textContent=mx.toLocaleString("fr-FR")+" ft";gAltMin=mn;gAltMax=mx;updateGlobeFlights();}
function globeFlyHome(){if(!gViewer)return;gViewer.camera.flyTo({destination:Cesium.Cartesian3.fromDegrees(10,46,8000000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.9,roll:0},duration:2});}
function tiltCamera(deg){if(!gViewer)return;const pitch=gViewer.camera.pitch+Cesium.Math.toRadians(deg);gViewer.camera.setView({orientation:{heading:gViewer.camera.heading,pitch:Math.max(-Cesium.Math.PI_OVER_TWO,Math.min(-0.05,pitch)),roll:0}});}
function resetNorth(){if(!gViewer)return;gViewer.camera.flyTo({destination:gViewer.camera.position,orientation:{heading:0,pitch:gViewer.camera.pitch,roll:0},duration:0.8});}
function flyToCity(name,lat,lon){if(!gViewer)return;gViewer.camera.flyTo({destination:Cesium.Cartesian3.fromDegrees(lon,lat,300000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.75,roll:0},duration:2.5});globeNotif("Navigation vers "+name,"#4fa3e0");}
function setDayMode(){if(!gViewer)return;gViewer.scene.globe.enableLighting=false;}
function setNightMode(){if(!gViewer)return;gViewer.scene.globe.enableLighting=true;}
function globeScreenshot(){if(!gViewer)return;gViewer.scene.render();const c=gViewer.scene.canvas.toDataURL("image/png");const a=document.createElement("a");a.download="godseye_globe_"+Date.now()+".png";a.href=c;a.click();globeNotif("Capture sauvegardée","#3a9a3a");}
function openStreetView(){
  const lat=sel?sel.latitude:selAP?selAP.lat:null,lon=sel?sel.longitude:selAP?selAP.lon:null;
  if(!lat)return globeNotif("Sélectionnez d'abord un élément","#b08828");
  window.open(`https://www.google.com/maps/@?api=1&map_action=pano&viewpoint=${lat},${lon}`,"_blank");
}

// Globe toggle control panel
function toggleGCtrl(){
  const p=document.getElementById("gctrl"),t=document.getElementById("gctrl-toggle");
  p.classList.toggle("open");t.classList.toggle("open");
  document.getElementById("globe-nav").classList.toggle("panel-open",p.classList.contains("open"));
}

// Globe search
document.getElementById("gc-search-input").addEventListener("keydown",e=>{if(e.key==="Enter")globeSearch();});
async function globeSearch(){
  const q=document.getElementById("gc-search-input").value.trim();if(!q)return;
  const res=document.getElementById("gc-search-results");
  res.innerHTML=`<div class="gsr-item"><div class="gsr-name">Recherche...</div></div>`;res.style.display="block";
  const apMatch=AIRPORTS.filter(a=>a.iata.includes(q.toUpperCase())||a.name.toLowerCase().includes(q.toLowerCase())||a.city.toLowerCase().includes(q.toLowerCase())).slice(0,3);
  let html=apMatch.map(a=>`<div class="gsr-item" onclick="pickAirport(AIRPORTS.find(x=>x.iata==='${a.iata}'))"><div class="gsr-name" style="color:var(--blu)">✈ ${a.iata} — ${a.name}</div><div class="gsr-sub">${a.city}, ${a.country}</div></div>`).join("");
  try{const r=await fetch(`https://nominatim.openstreetmap.org/search?q=${encodeURIComponent(q)}&format=json&limit=5&addressdetails=1`,{headers:{"Accept-Language":"fr"}});const d=await r.json();if(d?.length)html+=d.map(item=>`<div class="gsr-item" onclick="globeFlyToResult(${item.lat},${item.lon},'${item.display_name.split(',')[0].replace(/'/g,"\\'")}')"><div class="gsr-name">${item.display_name.split(',')[0]}</div><div class="gsr-sub">${item.display_name.split(',').slice(1,3).join(',').trim()}</div></div>`).join("");}catch(e){}
  res.innerHTML=html||`<div class="gsr-item"><div class="gsr-name">Aucun résultat</div></div>`;
}
function globeFlyToResult(lat,lon,name){
  document.getElementById("gc-search-results").style.display="none";
  document.getElementById("gc-search-input").value=name;
  if(gViewer)gViewer.camera.flyTo({destination:Cesium.Cartesian3.fromDegrees(parseFloat(lon),parseFloat(lat),50000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.8,roll:0},duration:2});
  globeNotif("Navigation vers "+name,"#4fa3e0");
}
document.addEventListener("click",e=>{if(!e.target.closest("#gctrl"))document.getElementById("gc-search-results").style.display="none";});

// Measure tool
function toggleMeasure(){
  measuringMode=!measuringMode;measurePts=[];
  const btn=document.getElementById("btn-measure");btn.classList.toggle("active",measuringMode);
  document.getElementById("measure-hint").style.display=measuringMode?"block":"none";
  if(!measuringMode){measureEnts.forEach(e=>{if(e&&!e.isDestroyed)gViewer.entities.remove(e);});measureEnts=[];if(gViewer)gViewer.scene.canvas.style.cursor="default";}
  else if(gViewer){gViewer.scene.canvas.style.cursor="crosshair";globeNotif("Mode mesure — cliquez 2 points","#4fa3e0");}
}
function addMeasurePoint(pos){
  const pt=Cesium.Cartesian3.fromDegrees(pos.longitude,pos.latitude,0);measurePts.push(pt);
  const pe=gViewer.entities.add({position:pt,point:{pixelSize:8,color:Cesium.Color.WHITE,outlineColor:Cesium.Color.BLACK,outlineWidth:1}});measureEnts.push(pe);
  if(measurePts.length===2){
    const d=Cesium.Cartesian3.distance(measurePts[0],measurePts[1]);
    const le=gViewer.entities.add({polyline:{positions:measurePts,width:2,material:new Cesium.PolylineDashMaterialProperty({color:Cesium.Color.WHITE})}});
    const mid=Cesium.Cartesian3.lerp(measurePts[0],measurePts[1],.5,new Cesium.Cartesian3());
    const la=gViewer.entities.add({position:mid,label:{text:d>1000?(d/1000).toFixed(0)+" km":Math.round(d)+" m",font:"bold 11px JetBrains Mono,monospace",fillColor:Cesium.Color.WHITE,outlineColor:Cesium.Color.BLACK,outlineWidth:2,style:Cesium.LabelStyle.FILL_AND_OUTLINE,disableDepthTestDistance:Number.POSITIVE_INFINITY,pixelOffset:new Cesium.Cartesian2(0,-16)}});
    measureEnts.push(le,la);
    document.getElementById("measure-hint").textContent=`Distance: ${d>1000?(d/1000).toFixed(0)+" km":Math.round(d)+" m"} — Cliquez pour nouvelle mesure`;
    measurePts=[];
  }
}

// ============================================================
// ===== MARITIME / AIS SYSTEM =====
// ============================================================

const AIS_STREAM_KEY = ""; // Clé gratuite sur aisstream.io

// ===== SHIP TYPE CONFIG =====
const SC={
  container: {c:"#4a90d9",mc:"#6aaef0",lbl:"Porte-conteneurs",shape:"container",ico:"C"},
  tanker:    {c:"#e8a020",mc:"#f0b840",lbl:"Petrolier",        shape:"tanker",   ico:"T"},
  lng:       {c:"#c8dc40",mc:"#ddf050",lbl:"Methanier LNG",    shape:"lng",      ico:"L"},
  bulk:      {c:"#9060c0",mc:"#b080e0",lbl:"Vraquier",         shape:"bulk",     ico:"B"},
  passenger: {c:"#40c8a0",mc:"#60e8c0",lbl:"Paquebot",         shape:"passenger",ico:"P"},
  fishing:   {c:"#60b840",mc:"#80d060",lbl:"Peche",            shape:"fishing",  ico:"F"},
  military:  {c:"#e04040",mc:"#ff6060",lbl:"Navire militaire", shape:"military", ico:"M"},
  yacht:     {c:"#d060d0",mc:"#f080f0",lbl:"Yacht prive",      shape:"yacht",    ico:"Y"},
  cargo:     {c:"#8090a0",mc:"#a0b0c0",lbl:"Cargo",            shape:"cargo",    ico:"G"},
  unknown:   {c:"#484848",mc:"#888888",lbl:"Inconnu",          shape:"unknown",  ico:"?"},
};
const NAV_STATUS=["En route (moteur)","Au mouillage","Hors service","Manoeuvre restreinte","Sous contrainte tirant d'eau","Amarre","Echoue","En peche","En route (voile)","Reserve","Reserve","Reserve","Reserve","Remorqueur","Remorque","Non defini"];
const PORT_TYPES={commercial:"#4a90d9",military:"#e04040",oil:"#e8a020",container:"#9060c0",mixed:"#40c8a0",lng:"#c8dc40"};

// ===== PORTS DATABASE (35 ports) =====
const PORTS=[
  // Europe
  {id:"RTM",name:"Rotterdam",country:"Pays-Bas",lat:51.9225,lon:4.47917,type:"container",cap:"469 Mt/an",terminals:42,depth:"23.5m",trafic:"469 Mt"},
  {id:"ANT",name:"Antwerpen",country:"Belgique",lat:51.2628,lon:4.3588,type:"container",cap:"271 Mt/an",terminals:22,depth:"16m",trafic:"271 Mt"},
  {id:"HAM",name:"Hamburg",country:"Allemagne",lat:53.5449,lon:9.9783,type:"container",cap:"130 Mt/an",terminals:8,depth:"17m",trafic:"130 Mt"},
  {id:"HAV",name:"Le Havre",country:"France",lat:49.4938,lon:0.1079,type:"container",cap:"90 Mt/an",terminals:5,depth:"15m",trafic:"90 Mt"},
  {id:"MRS",name:"Marseille-Fos",country:"France",lat:43.3398,lon:4.9583,type:"mixed",cap:"78 Mt/an",terminals:12,depth:"18m",trafic:"78 Mt"},
  {id:"BCN",name:"Barcelona",country:"Espagne",lat:41.3478,lon:2.1649,type:"mixed",cap:"70 Mt/an",terminals:8,depth:"16m",trafic:"70 Mt"},
  {id:"GEN",name:"Genova",country:"Italie",lat:44.4036,lon:8.9375,type:"mixed",cap:"63 Mt/an",terminals:11,depth:"14m",trafic:"63 Mt"},
  {id:"PIR",name:"Piraeus",country:"Grece",lat:37.9394,lon:23.6297,type:"container",cap:"55 Mt/an",terminals:3,depth:"17.5m",trafic:"55 Mt"},
  {id:"BRE",name:"Brest",country:"France",lat:48.3833,lon:-4.4833,type:"military",cap:"N/A",terminals:2,depth:"25m",trafic:"Militaire"},
  {id:"POR",name:"Portsmouth",country:"Royaume-Uni",lat:50.7988,lon:-1.0975,type:"military",cap:"N/A",terminals:4,depth:"18m",trafic:"Militaire"},
  // Middle East
  {id:"JAL",name:"Jebel Ali",country:"EAU",lat:24.9857,lon:55.0272,type:"container",cap:"180 Mt/an",terminals:3,depth:"17m",trafic:"180 Mt"},
  {id:"FUJ",name:"Fujairah",country:"EAU",lat:25.1288,lon:56.3394,type:"oil",cap:"190 Mt/an",terminals:8,depth:"22m",trafic:"190 Mt"},
  {id:"RAS",name:"Ras Tanura",country:"Arabie Saoudite",lat:26.6453,lon:50.1588,type:"oil",cap:"320 Mt/an",terminals:4,depth:"30m",trafic:"320 Mt"},
  {id:"BAB",name:"Bandar Abbas",country:"Iran",lat:27.1836,lon:56.2767,type:"mixed",cap:"88 Mt/an",terminals:5,depth:"14m",trafic:"88 Mt"},
  // Asia
  {id:"SIN",name:"Singapore",country:"Singapour",lat:1.2655,lon:103.8201,type:"container",cap:"585 Mt/an",terminals:7,depth:"20m",trafic:"585 Mt"},
  {id:"SHA",name:"Shanghai",country:"Chine",lat:31.2304,lon:121.4737,type:"container",cap:"750 Mt/an",terminals:19,depth:"17m",trafic:"750 Mt"},
  {id:"HKG",name:"Hong Kong",country:"Chine (SAR)",lat:22.2895,lon:114.1687,type:"container",cap:"196 Mt/an",terminals:9,depth:"17m",trafic:"196 Mt"},
  {id:"BUS",name:"Busan",country:"Coree du Sud",lat:35.1039,lon:129.0403,type:"container",cap:"219 Mt/an",terminals:8,depth:"18m",trafic:"219 Mt"},
  {id:"PKL",name:"Port Klang",country:"Malaisie",lat:3.0059,lon:101.3974,type:"container",cap:"130 Mt/an",terminals:3,depth:"17m",trafic:"130 Mt"},
  {id:"YOK",name:"Yokohama",country:"Japon",lat:35.4437,lon:139.6380,type:"mixed",cap:"128 Mt/an",terminals:12,depth:"15m",trafic:"128 Mt"},
  // Americas
  {id:"HOU",name:"Houston",country:"Etats-Unis",lat:29.7289,lon:-95.0146,type:"oil",cap:"277 Mt/an",terminals:10,depth:"14m",trafic:"277 Mt"},
  {id:"LAX",name:"Los Angeles/Long Beach",country:"Etats-Unis",lat:33.7541,lon:-118.2165,type:"container",cap:"190 Mt/an",terminals:12,depth:"16.5m",trafic:"190 Mt"},
  {id:"NYK",name:"New York/New Jersey",country:"Etats-Unis",lat:40.6893,lon:-74.0467,type:"mixed",cap:"140 Mt/an",terminals:6,depth:"15.5m",trafic:"140 Mt"},
  {id:"MIA",name:"Miami",country:"Etats-Unis",lat:25.7617,lon:-80.1918,type:"passenger",cap:"22 Mt/an",terminals:7,depth:"15m",trafic:"22 Mt"},
  {id:"SAN",name:"Santos",country:"Bresil",lat:-23.9519,lon:-46.3325,type:"container",cap:"114 Mt/an",terminals:14,depth:"15m",trafic:"114 Mt"},
  {id:"VAN",name:"Vancouver",country:"Canada",lat:49.2827,lon:-123.1207,type:"mixed",cap:"141 Mt/an",terminals:10,depth:"18m",trafic:"141 Mt"},
  // Africa / Strategic
  {id:"PSA",name:"Port Said",country:"Egypte",lat:31.2653,lon:32.3019,type:"mixed",cap:"58 Mt/an",terminals:5,depth:"19m",trafic:"58 Mt"},
  {id:"DJI",name:"Djibouti",country:"Djibouti",lat:11.5892,lon:43.1456,type:"mixed",cap:"29 Mt/an",terminals:3,depth:"18m",trafic:"29 Mt"},
  {id:"DUR",name:"Durban",country:"Afrique du Sud",lat:-29.8600,lon:31.0218,type:"mixed",cap:"90 Mt/an",terminals:8,depth:"16m",trafic:"90 Mt"},
  {id:"LAG",name:"Lagos (Apapa)",country:"Nigeria",lat:6.4531,lon:3.3958,type:"oil",cap:"70 Mt/an",terminals:6,depth:"13m",trafic:"70 Mt"},
];

// ===== STRATEGIC ZONES =====
const STRAT_ZONES=[
  {name:"Detroit d'Ormuz",minLat:26.0,maxLat:26.8,minLon:56.1,maxLon:57.4,color:"rgba(220,50,50,.15)",border:"rgba(220,50,50,.6)"},
  {name:"Canal de Suez",minLat:30.0,maxLat:31.5,minLon:32.3,maxLon:32.7,color:"rgba(220,150,30,.13)",border:"rgba(220,150,30,.6)"},
  {name:"Bab el-Mandeb",minLat:11.5,maxLat:13.5,minLon:42.5,maxLon:44.5,color:"rgba(220,50,50,.15)",border:"rgba(220,50,50,.6)"},
  {name:"Detroit de Malacca",minLat:1.0,maxLat:5.5,minLon:99.5,maxLon:104.0,color:"rgba(80,160,220,.13)",border:"rgba(80,160,220,.6)"},
  {name:"Bosphore",minLat:40.8,maxLat:41.5,minLon:28.7,maxLon:29.2,color:"rgba(180,80,220,.13)",border:"rgba(180,80,220,.5)"},
  {name:"Canal de Panama",minLat:8.7,maxLat:9.5,minLon:-79.9,maxLon:-78.9,color:"rgba(220,150,30,.13)",border:"rgba(220,150,30,.6)"},
];

// ===== MARITIME STATE =====
let vessels=[],selShip=null,selPort=null,hovShip=null,hovPort=null;
let shipTrails={};
let showShips=true,showPorts=true,showZones=true;
let gShipEntities={},gPortEntities={},gZoneEntities=[];
let aisWs=null,aisConnected=false;
let activeShTab="info",activePortTab="overview";
let lastShAI="",shipAiOpen=false;
let vesselInt=null;

// ===== SHIP CLASSIFICATION =====
function classifyVessel(v){
  const t=parseInt(v.typeCode||"0"),name=(v.name||"").toUpperCase();
  if(t>=35&&t<=37||/WARSHIP|HMS |USS |FNS |NRP |HNLMS/.test(name))return"military";
  if(t>=60&&t<=69||/FERRY|CRUISE|QUEEN|CARNIVAL|MSC MAGNIFICA/.test(name))return"passenger";
  if(t>=70&&t<=79||/MSC |CMA |EVERGREEN|COSCO|HAPAG|MAERSK/.test(name))return"container";
  if(t>=80&&t<=89||/SUEZMAX|VLCC|AFRAMAX|TANKER/.test(name))return"tanker";
  if(t>=70&&t<=89&&/LNG|GAS|ARCTIC/.test(name))return"lng";
  if(/BULK|CAPESIZE|PANAMAX|HANDYMAX/.test(name))return"bulk";
  if(t>=30&&t<=33||/FISHER/.test(name))return"fishing";
  if(t>=36&&t<=37||/YACHT|SAIL/.test(name))return"yacht";
  if(t>=70)return"cargo";
  return"unknown";
}
const fKts=k=>k!=null?k.toFixed(1)+" kn":"N/A";
const fDeg2=d=>d!=null?Math.round(d)+"deg":"N/A";

// ===== DEMO VESSEL GENERATION =====
function genDemoVessels(b){
  // Maritime zones per region: pre-defined shipping lane points
  const zones={
    europe:[
      {lat:51.5,lon:3.5,type:"container",name:"MSC OSCAR"},    // North Sea
      {lat:51.0,lon:1.5,type:"container",name:"CMA CGM MARCO POLO"},
      {lat:50.2,lon:-1.2,type:"passenger",name:"P&O AURORA"},   // Channel
      {lat:49.5,lon:-4.0,type:"cargo",name:"CELTIC HORIZON"},
      {lat:48.5,lon:-6.5,type:"tanker",name:"NORDIC OLYMPIC"},  // Atlantic
      {lat:44.0,lon:-5.0,type:"tanker",name:"ATLANTIC NAVIGATOR"},
      {lat:43.5,lon:5.5,type:"cargo",name:"COTE D'AZUR"},       // Med
      {lat:40.5,lon:14.5,type:"container",name:"MAERSK SEALAND"},
      {lat:37.5,lon:23.0,type:"bulk",name:"AEGEAN GLORY"},
      {lat:41.0,lon:29.0,type:"tanker",name:"ISTANBUL STAR"},
      {lat:55.5,lon:10.5,type:"cargo",name:"NORDIC ORESUND"},   // Baltic
      {lat:58.0,lon:5.0,type:"fishing",name:"SOLVORN"},
      {lat:45.5,lon:13.5,type:"cargo",name:"ADRIATIC HAWK"},
      {lat:36.0,lon:-5.0,type:"container",name:"GIBRALTAR EXPRESS"}, // Gibraltar
      {lat:38.5,lon:-9.5,type:"tanker",name:"LUSITANIA SPIRIT"},
      {lat:59.5,lon:3.5,type:"bulk",name:"NORTH SEA GIANT"},
    ],
    france:[
      {lat:50.8,lon:2.0,type:"container",name:"LE HAVRE STAR"},
      {lat:49.4,lon:-1.8,type:"cargo",name:"BRITTANY FERRIES"},
      {lat:48.5,lon:-5.5,type:"fishing",name:"CAP SIZUN"},
      {lat:47.0,lon:-3.5,type:"tanker",name:"LOIRE SPIRIT"},
      {lat:44.0,lon:-2.0,type:"cargo",name:"BASQUE CARRIER"},
      {lat:43.0,lon:4.5,type:"container",name:"MARSEILLE EXPRESS"},
    ],
    northamerica:[
      {lat:40.5,lon:-72.5,type:"container",name:"EVER GIVEN"},    // East coast
      {lat:38.5,lon:-75.0,type:"tanker",name:"GULF FALCON"},
      {lat:29.0,lon:-93.5,type:"tanker",name:"TEXACO SPIRIT"},    // Gulf
      {lat:26.0,lon:-87.0,type:"container",name:"SEALAND EAGLE"},
      {lat:33.5,lon:-118.5,type:"container",name:"APL HONGKONG"}, // West coast
      {lat:47.0,lon:-122.5,type:"bulk",name:"COLUMBIA QUEEN"},
      {lat:44.0,lon:-60.0,type:"cargo",name:"ATLANTIC COMPANION"},// Atlantic
      {lat:35.0,lon:-72.0,type:"passenger",name:"CARNIVAL GLORY"},
      {lat:24.0,lon:-81.0,type:"passenger",name:"HARMONY OF THE SEAS"},
      {lat:42.0,lon:-66.0,type:"fishing",name:"GRAND BANKS"},
      {lat:56.0,lon:-52.0,type:"bulk",name:"ARCTIC VIKING"},
    ],
    world:[
      {lat:25.2,lon:55.5,type:"tanker",name:"GULF TITAN"},        // Persian Gulf
      {lat:26.5,lon:56.3,type:"tanker",name:"HORMUZ GLORY"},
      {lat:12.5,lon:44.0,type:"cargo",name:"BAB EL MANDEB"},      // Bab el-Mandeb
      {lat:1.5,lon:103.5,type:"container",name:"SINGAPORE EXPRESS"}, // Malacca
      {lat:3.0,lon:101.0,type:"container",name:"COSCO SHIPPING"},
      {lat:31.2,lon:32.5,type:"container",name:"SUEZ NAVIGATOR"}, // Suez
      {lat:22.5,lon:115.0,type:"container",name:"SHANGHAI EXPRESS"}, // S. China Sea
      {lat:35.0,lon:139.5,type:"container",name:"PACIFIC LUNA"},  // Japan
      {lat:-33.0,lon:17.5,type:"bulk",name:"CAPE OF GOOD HOPE"},  // Cape
      {lat:-25.0,lon:-43.5,type:"container",name:"SANTOS STAR"},  // Brazil
      {lat:51.0,lon:-35.0,type:"container",name:"ATLANTIC BRIDGE"}, // N. Atlantic
      {lat:10.0,lon:60.0,type:"tanker",name:"ARABIAN SEA GLORY"}, // Arabian Sea
      {lat:-8.0,lon:85.0,type:"bulk",name:"INDIAN OCEAN CARRIER"},
      {lat:15.0,lon:-30.0,type:"tanker",name:"W.AFRICAN SPIRIT"}, // W. Africa
      {lat:41.5,lon:29.5,type:"container",name:"BOSPHORUS STAR"}, // Bosphorus
      {lat:9.0,lon:-79.5,type:"container",name:"PANAMA BLUE"},    // Panama
      {lat:-37.0,lon:150.0,type:"bulk",name:"SYDNEY CARRIER"},    // Australia
      {lat:60.0,lon:5.5,type:"fishing",name:"ARKTIS TROLL"},      // Norway
      {lat:4.0,lon:-0.5,type:"tanker",name:"GULF OF GUINEA"},     // W. Africa coast
      {lat:26.0,lon:-78.0,type:"passenger",name:"FREEDOM OF SEAS"}, // Caribbean
    ],
  };
  const zone=zones[region]||zones.europe;
  const names=["SEAWISE GIANT","VALOR","PIONEER","ENTERPRISE","HORIZON","DISCOVERY","EXPLORER","HERITAGE","LEGEND","FORTUNA","ENDEAVOR","PROGRESS","TRIUMPH","PACIFIC","ATLANTIC","NORDIC","BALTIC","ARCTIC","AEGEAN","MARINE"];
  return zone.map((z,i)=>{
    const sc=SC[z.type]||SC.unknown;
    const noiseLat=(Math.random()-.5)*2,noiseLon=(Math.random()-.5)*3;
    const lat=Math.max(b.minLat+1,Math.min(b.maxLat-1,z.lat+noiseLat));
    const lon=Math.max(b.minLon+1,Math.min(b.maxLon-1,z.lon+noiseLon));
    const speed=z.type==="fishing"?3+Math.random()*8:z.type==="passenger"?12+Math.random()*10:z.type==="tanker"||z.type==="bulk"?8+Math.random()*7:z.type==="military"?15+Math.random()*15:10+Math.random()*8;
    const hdg=Math.random()*360,t=z.type;
    const len=t==="container"?200+~~(Math.random()*200):t==="tanker"||t==="lng"?200+~~(Math.random()*180):t==="bulk"?150+~~(Math.random()*150):t==="passenger"?200+~~(Math.random()*100):t==="military"?80+~~(Math.random()*120):t==="fishing"?20+~~(Math.random()*40):50+~~(Math.random()*100);
    const w=~~(len*0.14)+5;
    const mmsi=String(200000000+Math.floor(Math.random()*800000000)).slice(0,9);
    const imo="9"+String(Math.floor(Math.random()*9000000)+1000000);
    const navSt=speed<0.5?1:Math.random()<.05?5:0;
    const hasAisGap=Math.random()<0.05;
    return{
      mmsi,imo,callsign:"V7"+String(~~(Math.random()*9000)+1000),
      name:z.name||(names[i%names.length]+" "+String(i+1).padStart(2,"0")),
      latitude:lat,longitude:lon,
      heading:hdg,cog:hdg+(Math.random()-.5)*5,
      speed,typeCode:t==="container"?"70":t==="tanker"?"80":t==="passenger"?"60":t==="fishing"?"30":t==="military"?"35":"79",
      type:t,length:len,width:w,draft:t==="tanker"?15+Math.random()*8:t==="container"?12+Math.random()*3:8+Math.random()*6,
      flag:["NL","DE","FR","GB","GR","IT","CY","BS","PA","MH","LR"][~~(Math.random()*11)],
      destination:["ROTTERDAM","HAMBURG","SINGAPORE","SHANGHAI","DUBAI","MARSEILLE","HOUSTON","ANTWERP"][~~(Math.random()*8)],
      eta:"2026-03-"+String(15+~~(Math.random()*14)).padStart(2,"0")+" "+String(~~(Math.random()*24)).padStart(2,"0")+":00",
      navStatus:navSt,
      last_contact:Math.floor(Date.now()/1000)-~~(Math.random()*600),
      hasAisGap,anomalies:[],
    };
  });
}

// ===== AIS FETCH =====
async function loadVessels(){
  const b=REGIONS[region];
  if(aisConnected&&aisWs?.readyState===WebSocket.OPEN){return;} // live feed active
  vessels=genDemoVessels(b);
  vessels.forEach(v=>{
    if(!shipTrails[v.mmsi])shipTrails[v.mmsi]=[];
    shipTrails[v.mmsi].push({lat:v.latitude,lon:v.longitude});
    if(shipTrails[v.mmsi].length>20)shipTrails[v.mmsi].shift();
  });
  // Simulate slow movement
  vessels.forEach(v=>{
    const spd=(v.speed||8)/111000*30; // ~30s tick equivalent
    const r=(v.heading||0)*Math.PI/180;
    v.latitude=Math.max(b.minLat+.2,Math.min(b.maxLat-.2,v.latitude+Math.cos(r)*spd));
    v.longitude=Math.max(b.minLon+.2,Math.min(b.maxLon-.2,v.longitude+Math.sin(r)*spd));
    // Anomaly detection
    v.anomalies=detectAnomalies(v);
  });
  updateMaritimeStats();
  drawMap();
  if(globeInited)updateGlobeShips();
}

// AISStream.io WebSocket
function connectAISStream(){
  const key=(document.getElementById("ais-key").value.trim()||CFG.aisStream);CFG.aisStream=key;saveCfgLS();
  if(!key){showGlobeNotif("Entrez votre clé AISStream.io","#c44040");return;}
  if(aisWs)aisWs.close();
  document.getElementById("ais-status").textContent="Connexion...";
  aisWs=new WebSocket("wss://stream.aisstream.io/v0/stream");
  const b=REGIONS[region];
  aisWs.onopen=()=>{
    aisWs.send(JSON.stringify({APIKey:key,BoundingBoxes:[[[b.minLat,b.minLon],[b.maxLat,b.maxLon]]],FilterMessageTypes:["PositionReport","ShipStaticData"]}));
    aisConnected=true;document.getElementById("ais-status").textContent="Connecte - AIS live";
    document.getElementById("ais-status").style.color="var(--grn)";
  };
  aisWs.onmessage=ev=>{
    try{
      const msg=JSON.parse(ev.data);
      const meta=msg.MetaData||{};
      const mmsi=String(meta.MMSI||"");
      if(!mmsi)return;
      let v=vessels.find(x=>x.mmsi===mmsi);
      if(!v){v={mmsi,name:meta.ShipName||"UNKNOWN",latitude:meta.latitude||0,longitude:meta.longitude||0,heading:0,cog:0,speed:0,type:"unknown",flag:"",destination:"",navStatus:0,length:0,width:0,draft:0,imo:"",callsign:"",anomalies:[]};vessels.push(v);}
      if(msg.MessageType==="PositionReport"){const p=msg.Message.PositionReport;v.latitude=p.Latitude;v.longitude=p.Longitude;v.heading=p.TrueHeading||p.Cog||0;v.cog=p.Cog||0;v.speed=p.Sog||0;v.navStatus=p.NavigationalStatus||0;v.last_contact=Date.now()/1000;}
      if(msg.MessageType==="ShipStaticData"){const s=msg.Message.ShipStaticData;v.name=s.Name||v.name;v.imo="9"+String(s.ImoNumber||"");v.callsign=s.CallSign||"";v.destination=s.Destination||"";v.eta=s.Eta||"";v.length=(s.Dimension?.A||0)+(s.Dimension?.B||0);v.width=(s.Dimension?.C||0)+(s.Dimension?.D||0);v.draft=s.MaximumStaticDraught||0;v.typeCode=String(s.Type||"79");v.type=classifyVessel(v);}
      if(!shipTrails[mmsi])shipTrails[mmsi]=[];shipTrails[mmsi].push({lat:v.latitude,lon:v.longitude});if(shipTrails[mmsi].length>20)shipTrails[mmsi].shift();
      v.anomalies=detectAnomalies(v);
      updateMaritimeStats();drawMap();if(globeInited)updateGlobeShips();
    }catch(e){}
  };
  aisWs.onerror=()=>{document.getElementById("ais-status").textContent="Erreur connexion";document.getElementById("ais-status").style.color="var(--red)";};
  aisWs.onclose=()=>{aisConnected=false;};
}
function showGlobeNotif(m,c){if(globeInited)globeNotif(m,c);}

// ===== ANOMALY DETECTION =====
function detectAnomalies(v){
  const a=[];
  if(v.hasAisGap)a.push({type:"crit",msg:"Signal AIS perdu — position non certifiee"});
  if(v.navStatus===2)a.push({type:"crit",msg:"Navire hors service (NUC)"});
  if(v.navStatus===1&&v.speed>3)a.push({type:"warn",msg:"Vitesse anormale au mouillage"});
  if(v.type==="tanker"&&v.speed>18)a.push({type:"warn",msg:"Vitesse excessive pour un petrolier"});
  if(v.type==="container"&&v.speed>25)a.push({type:"warn",msg:"Vitesse excessive (>25 kn)"});
  if(v.speed>0.2&&v.speed<0.5&&v.navStatus===0)a.push({type:"info",msg:"Vitesse tres faible en route — derive possible"});
  // Strategic zones check
  const zones=getZonesNearPos(v.latitude,v.longitude);
  zones.forEach(z=>a.push({type:"info",msg:"Proximite : "+z.name}));
  if(v.type==="military")a.push({type:"warn",msg:"Navire militaire — surveillance recommandee"});
  return a;
}
function getZonesNearPos(lat,lon){
  const margin=1.5;
  return STRAT_ZONES.filter(z=>lat>z.minLat-margin&&lat<z.maxLat+margin&&lon>z.minLon-margin&&lon<z.maxLon+margin);
}

// ===== 2D MAP DRAWING — STRATEGIC ZONES =====
function drawStrategicZones(ctx,b,w,h){
  STRAT_ZONES.forEach(z=>{
    if(z.maxLat<b.minLat||z.minLat>b.maxLat||z.maxLon<b.minLon||z.minLon>b.maxLon)return;
    const p1=ll2xy(z.maxLat,z.minLon,b,w,h),p2=ll2xy(z.minLat,z.maxLon,b,w,h);
    const rx=Math.min(p1.x,p2.x),ry=Math.min(p1.y,p2.y),rw=Math.abs(p2.x-p1.x)||8,rh=Math.abs(p2.y-p1.y)||8;
    if(rw<2||rh<2)return;
    ctx.fillStyle=z.color;ctx.fillRect(rx,ry,rw,rh);
    ctx.strokeStyle=z.border;ctx.lineWidth=1;ctx.setLineDash([3,3]);
    ctx.strokeRect(rx,ry,rw,rh);ctx.setLineDash([]);
    ctx.font="bold 8px 'JetBrains Mono',monospace";ctx.fillStyle=z.border;
    ctx.fillText(z.name,rx+3,ry+10);
  });
}

// ===== 2D MAP DRAWING — PORTS =====
function drawPortsOnMap(ctx,b,w,h){
  if(!showPorts)return;
  PORTS.forEach(p=>{
    if(p.lat<b.minLat||p.lat>b.maxLat||p.lon<b.minLon||p.lon>b.maxLon)return;
    const{x,y}=ll2xy(p.lat,p.lon,b,w,h);
    const isSel=selPort?.id===p.id,isHov=hovPort?.id===p.id,sz=isSel||isHov?8:5;
    const tc=PORT_TYPES[p.type]||"#d4a020";
    ctx.save();ctx.translate(x,y);ctx.rotate(Math.PI/4); // diamond
    ctx.fillStyle=isSel?tc:isHov?tc+"dd":"rgba(212,160,32,.5)";
    ctx.strokeStyle=isSel?tc:"rgba(212,160,32,.8)";ctx.lineWidth=1;
    ctx.shadowColor=tc;ctx.shadowBlur=isSel?12:isHov?6:2;
    ctx.fillRect(-sz,-sz,sz*2,sz*2);ctx.strokeRect(-sz,-sz,sz*2,sz*2);
    ctx.restore();ctx.shadowBlur=0;
    if(isSel||isHov||(region!=="world"&&region!=="northamerica")){
      ctx.font=`${isSel?"bold ":""}8px 'JetBrains Mono',monospace`;
      ctx.fillStyle=isSel?tc:"rgba(212,160,32,.7)";
      ctx.fillText(p.name.split(" ")[0],x+sz+3,y+3);
    }
  });
}

// ===== 2D MAP DRAWING — SHIPS =====
function drawShipsOnMap(ctx,b,w,h){
  if(!showShips)return;
  vessels.forEach(v=>{
    if(v.latitude<b.minLat||v.latitude>b.maxLat||v.longitude<b.minLon||v.longitude>b.maxLon)return;
    const{x,y}=ll2xy(v.latitude,v.longitude,b,w,h);
    const sc=SC[v.type]||SC.unknown,isSel=selShip?.mmsi===v.mmsi,isHov=hovShip?.mmsi===v.mmsi;
    const hasAno=v.anomalies?.some(a=>a.type==="crit"||a.type==="warn");
    // Trail
    const tr=shipTrails[v.mmsi]||[];
    if(tr.length>1){ctx.beginPath();tr.forEach((p,i)=>{const tp=ll2xy(p.lat,p.lon,b,w,h);i===0?ctx.moveTo(tp.x,tp.y):ctx.lineTo(tp.x,tp.y);});ctx.strokeStyle=sc.c+"44";ctx.lineWidth=1.5;ctx.setLineDash([2,3]);ctx.stroke();ctx.setLineDash([]);}
    if(isSel){ctx.beginPath();ctx.arc(x,y,15,0,Math.PI*2);ctx.strokeStyle=sc.c+"55";ctx.lineWidth=1.5;ctx.stroke();}
    // Ship hull (top-down silhouette)
    const baseLen=Math.max(7,Math.min(18,v.length/18));
    const baseW=Math.max(3,baseLen*0.35);
    const sz=isSel?baseLen*1.3:isHov?baseLen*1.15:baseLen;
    const sw=sz*0.35;
    ctx.save();ctx.translate(x,y);ctx.rotate(((v.heading||v.cog||0)*Math.PI)/180);
    // Bow = pointed top, stern = flat bottom
    ctx.beginPath();ctx.moveTo(0,-sz);ctx.lineTo(sw,-sz*.3);ctx.lineTo(sw,sz*.6);ctx.lineTo(-sw,sz*.6);ctx.lineTo(-sw,-sz*.3);ctx.closePath();
    ctx.fillStyle=isSel?"#ffffff":sc.mc;ctx.shadowColor=sc.mc;ctx.shadowBlur=isSel?10:hasAno?6:3;ctx.fill();ctx.restore();
    if(hasAno&&!isSel){ctx.font="bold 8px sans-serif";ctx.fillStyle="#e04040";ctx.fillText("!",x-3,y-sz-3);}
    if(isSel||isHov){ctx.font="500 10px 'Inter',sans-serif";ctx.fillStyle=sc.mc;ctx.shadowColor=sc.mc;ctx.shadowBlur=4;ctx.fillText(v.name.slice(0,18),x+sz+4,y+3);ctx.shadowBlur=0;}
  });
}

// ===== 2D MAP EVENTS — extend mousemove with ship/port hit test =====
cvs.addEventListener("mousemove",e=>{
  if(currentView!=="2d")return;
  const rc=cvs.getBoundingClientRect(),mx=e.clientX-rc.left,my=e.clientY-rc.top;
  const b=REGIONS[region],w=cvs.width,hh=cvs.height;
  let cs=null,csd=14;
  vessels.forEach(v=>{if(v.latitude<b.minLat||v.latitude>b.maxLat||v.longitude<b.minLon||v.longitude>b.maxLon)return;const{x,y}=ll2xy(v.latitude,v.longitude,b,w,hh);const d=Math.hypot(x-mx,y-my);if(d<csd){csd=d;cs=v;}});
  let cp=null,cpd=12;
  PORTS.forEach(p=>{if(p.lat<b.minLat||p.lat>b.maxLat||p.lon<b.minLon||p.lon>b.maxLon)return;const{x,y}=ll2xy(p.lat,p.lon,b,w,hh);const d=Math.hypot(x-mx,y-my);if(d<cpd){cpd=d;cp=p;}});
  if(hovShip!==cs||hovPort!==cp){hovShip=cs;hovPort=cp;drawMap();}
  const tip=document.getElementById("htip");
  if(cs&&csd<12&&!selShip&&!hov&&!hovAP){const sc=SC[cs.type]||SC.unknown;tip.style.display="block";tip.innerHTML=`<strong style="color:${sc.c}">${cs.name}</strong>  <span style="color:var(--t3);font-size:10px">${cs.flag||""} — ${sc.lbl}</span><br><span style="color:var(--t3)">${cs.speed?.toFixed(1)||0} kn  |  ${cs.length}m  |  Cap ${Math.round(cs.heading||0)}deg  |  ${cs.destination||"N/A"}</span>`;}
  else if(cp&&cpd<10&&!selPort&&!hov&&!hovAP&&!cs){tip.style.display="block";tip.innerHTML=`<strong style="color:#d4a020">${cp.name}</strong>  <span style="color:var(--t3);font-size:10px">${cp.country}</span><br><span style="color:var(--t3)">${cp.type}  |  Cap: ${cp.cap}  |  Tirant: ${cp.depth}</span>`;}
});

// ===== PICK SHIP =====
function pickShip(v){
  selShip=v;sel=null;selAP=null;selPort=null;
  renderList();drawMap();centerOn(v.latitude,v.longitude);
  setShipMode();openDet();
  document.querySelectorAll(".det-tab-dot").forEach(d=>d.style.background=SC[v.type]?.c||"#8090a0");
  document.querySelector(".det-tab-txt").textContent="NAVIRE";
  // Header
  document.getElementById("sh-name").textContent=v.name;
  document.getElementById("sh-mmsi").textContent=`MMSI ${v.mmsi}  |  IMO ${v.imo||"N/A"}  |  ${v.callsign||""}`;
  const sc=SC[v.type]||SC.unknown,b=document.getElementById("sh-badge");
  b.textContent=sc.lbl.toUpperCase();b.style.color=sc.c;b.style.borderColor=sc.c+"55";b.style.background=sc.c+"11";
  document.getElementById("sh-flag").textContent=`Pavillon: ${v.flag||"N/A"}`;
  document.getElementById("sh-spd").textContent=fKts(v.speed);
  document.getElementById("sh-hdg").textContent=fDeg2(v.heading);
  document.getElementById("sh-stat-nav").textContent=NAV_STATUS[v.navStatus||0]||"En route";
  // Ident tab
  const kv=(id,rows)=>{document.getElementById(id).innerHTML=rows.map(([k,val])=>`<div class="ap-kv-row"><div>${k}</div><div>${val||"N/A"}</div></div>`).join("");};
  kv("sh-ident",[["Nom",v.name],["MMSI",v.mmsi],["IMO",v.imo||"N/A"],["Indicatif radio",v.callsign||"N/A"],["Pavillon",v.flag||"N/A"],["Type AIS",v.typeCode+` (${sc.lbl})`]]);
  kv("sh-chars",[["Longueur",v.length?""+v.length+" m":"N/A"],["Largeur",v.width?""+v.width+" m":"N/A"],["Tirant d'eau",v.draft?v.draft.toFixed(1)+" m":"N/A"],["Categorie",sc.lbl]]);
  document.getElementById("sh-mt-link").href=`https://www.marinetraffic.com/en/ais/details/ships/mmsi:${v.mmsi}`;
  document.getElementById("sh-vf-link").href=`https://www.vesselfinder.com/?mmsi=${v.mmsi}`;
  // Nav tab
  kv("sh-nav-data",[["Vitesse sol (SOG)",fKts(v.speed)],["Cap vrai (HDG)",fDeg2(v.heading)],["Route fond (COG)",fDeg2(v.cog)],["Statut nav.",NAV_STATUS[v.navStatus||0]],["Latitude",v.latitude?.toFixed(5)],["Longitude",v.longitude?.toFixed(5)],["Dernier contact",ago(v.last_contact)]]);
  drawShipZoom();
  // Voyage tab
  document.getElementById("sh-dep").textContent="???";
  document.getElementById("sh-dest").textContent=v.destination?v.destination.slice(0,4):"???";
  document.getElementById("sh-dest-name").textContent=v.destination||"Non renseigne";
  const sp=v.speed>0?Math.round(Math.random()*80):0;
  document.getElementById("sh-pb").style.width=sp+"%";
  document.getElementById("sh-progress-lbl").textContent=sp+"% parcouru (estime)";
  kv("sh-voyage-data",[["Destination",v.destination||"N/A"],["ETA",v.eta||"N/A"],["Vitesse actuelle",fKts(v.speed)],["Statut",NAV_STATUS[v.navStatus||0]]]);
  const pts=shipTrails[v.mmsi]||[];
  document.getElementById("sh-trail-list").innerHTML=pts.slice(-6).reverse().map((p,i)=>`<div style="padding:3px 0;border-bottom:1px solid var(--b1);color:var(--t2)">${i===0?"NOW":ago(v.last_contact-(i*120))+" ago"}  —  ${p.lat.toFixed(4)}° / ${p.lon.toFixed(4)}°</div>`).join("")||"Aucun historique";
  // Anomalies tab
  const aList=document.getElementById("sh-anomalies-list");
  aList.innerHTML=v.anomalies?.length?v.anomalies.map(a=>`<div class="ano-badge ${a.type}">${a.type==="crit"?"CRITIQUE":a.type==="warn"?"ALERTE":"INFO"} — ${a.msg}</div>`).join(""):`<div style="font-size:10px;color:var(--t3);padding:6px 0">Aucune anomalie detectee</div>`;
  const near=getZonesNearPos(v.latitude,v.longitude);
  document.getElementById("sh-zones-list").innerHTML=near.length?near.map(z=>`<div class="ano-badge info">${z.name}</div>`).join(""):
    `<div style="font-size:10px;color:var(--t3)">Aucune zone strategique a proximite immediate</div>`;
  switchShTab("info");
  // Globe fly
  if(globeInited&&gViewer)gViewer.camera.flyTo({destination:Cesium.Cartesian3.fromDegrees(v.longitude,v.latitude,80000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.75,roll:0},duration:2});
}

// ===== PICK PORT =====
function pickPort(p){
  selPort=p;selShip=null;sel=null;selAP=null;
  drawMap();centerOn(p.lat,p.lon);setPortMode();openDet();
  document.querySelectorAll(".det-tab-dot").forEach(d=>d.style.background="#d4a020");
  document.querySelector(".det-tab-txt").textContent="PORT";
  document.getElementById("port-name").textContent=p.name;
  document.getElementById("port-ctry").textContent=`${p.country}  |  ${p.lat.toFixed(3)}N, ${p.lon.toFixed(3)}E`;
  const pb=document.getElementById("port-badge"),tc=PORT_TYPES[p.type]||"#d4a020";
  pb.textContent=p.type.toUpperCase();pb.style.color=tc;pb.style.borderColor=tc+"55";pb.style.background=tc+"11";
  document.getElementById("port-cap").textContent=p.cap;
  document.getElementById("port-term").textContent=String(p.terminals);
  document.getElementById("port-depth").textContent=p.depth;
  // Ident section
  document.getElementById("port-ident").innerHTML=[["Nom",p.name],["Pays",p.country],["Type",p.type],["Latitude",p.lat.toFixed(4)+"°"],["Longitude",p.lon.toFixed(4)+"°"]].map(([k,v])=>`<div class="ap-kv-row"><div>${k}</div><div>${v}</div></div>`).join("");
  document.getElementById("port-infra").innerHTML=[["Capacite annuelle",p.cap||"-"],["Terminaux",String(p.terminals)],["Tirant d'eau max",p.depth],["Trafic annuel",p.trafic||"-"]].map(([k,v])=>`<div class="ap-kv-row"><div>${k}</div><div>${v}</div></div>`).join("");
  // Traffic tab: ships nearby
  const nearby=vessels.filter(v=>Math.abs(v.latitude-p.lat)<0.6&&Math.abs(v.longitude-p.lon)<0.6);
  const sums=nearby.reduce((acc,v)=>{acc[v.type]=(acc[v.type]||0)+1;return acc;},{});
  document.getElementById("port-traffic-list").innerHTML=nearby.length?nearby.slice(0,8).map(v=>{const sc=SC[v.type]||SC.unknown;return`<div style="padding:4px 0;border-bottom:1px solid var(--b1);display:flex;align-items:center;gap:8px;cursor:pointer" onclick="pickShip(vessels.find(x=>x.mmsi==='${v.mmsi}'))"><div style="width:6px;height:6px;border-radius:50%;background:${sc.c};flex-shrink:0"></div><div style="flex:1;font-family:'JetBrains Mono',monospace;font-size:10px;color:var(--t1)">${v.name.slice(0,20)}</div><div style="font-size:9px;color:${sc.c}">${sc.lbl}</div></div>`;}).join(""):`<div style="font-size:10px;color:var(--t4);padding:6px 0">Aucun navire AIS detecte dans un rayon de 60km</div>`;
  document.getElementById("port-traffic-stats").innerHTML=[["Navires detectes",String(nearby.length)],["Types",(Object.entries(sums).map(([k,n])=>`${n} ${(SC[k]||SC.unknown).lbl}`).join(", "))||"N/A"]].map(([k,v])=>`<div class="ap-kv-row"><div>${k}</div><div>${v}</div></div>`).join("");
  switchPortTab("overview");
  loadPortAI(p);
  if(globeInited&&gViewer)gViewer.camera.flyTo({destination:Cesium.Cartesian3.fromDegrees(p.lon,p.lat,100000),orientation:{heading:0,pitch:-Cesium.Math.PI_OVER_TWO*.8,roll:0},duration:2});
}

// ===== SHIP ZOOM CANVAS =====
function drawShipZoom(){
  const v=selShip;if(!v)return;
  const zcan=document.getElementById("sh-zc"),zw=zcan.clientWidth||294;
  zcan.width=zw;zcan.height=110;
  const zx=zcan.getContext("2d"),pts=shipTrails[v.mmsi]||[],sc=SC[v.type]||SC.unknown;
  const mg=0.5,allPts=[...pts,{lat:v.latitude,lon:v.longitude}];
  let mnLat=v.latitude-mg,mxLat=v.latitude+mg,mnLon=v.longitude-mg*1.5,mxLon=v.longitude+mg*1.5;
  if(pts.length>1){mnLat=Math.min(...allPts.map(p=>p.lat))-.15;mxLat=Math.max(...allPts.map(p=>p.lat))+.15;mnLon=Math.min(...allPts.map(p=>p.lon))-.15;mxLon=Math.max(...allPts.map(p=>p.lon))+.15;}
  const zb={minLat:mnLat,maxLat:mxLat,minLon:mnLon,maxLon:mxLon};
  zx.fillStyle="#060606";zx.fillRect(0,0,zw,110);
  zx.strokeStyle="rgba(255,255,255,.02)";zx.lineWidth=1;
  for(let i=0;i<zw;i+=28){zx.beginPath();zx.moveTo(i,0);zx.lineTo(i,110);zx.stroke();}
  for(let j=0;j<110;j+=28){zx.beginPath();zx.moveTo(0,j);zx.lineTo(zw,j);zx.stroke();}
  if(pts.length>1){zx.beginPath();pts.forEach((p,i)=>{const{x,y}=ll2xy(p.lat,p.lon,zb,zw,110);i===0?zx.moveTo(x,y):zx.lineTo(x,y);});zx.strokeStyle=sc.c+"88";zx.lineWidth=1.5;zx.setLineDash([3,3]);zx.stroke();zx.setLineDash([]);}
  const cx=zw/2,cy=55,rd=((v.heading||0)*Math.PI)/180,sz=7;
  zx.save();zx.translate(cx,cy);zx.rotate(rd);
  zx.fillStyle="#ffffff";zx.shadowColor=sc.mc;zx.shadowBlur=8;
  zx.beginPath();zx.moveTo(0,-sz);zx.lineTo(sz*.38,sz*.62);zx.lineTo(0,sz*.2);zx.lineTo(-sz*.38,sz*.62);zx.closePath();zx.fill();zx.restore();
  // SOG vector
  if(v.speed>0.5){const spd=Math.min(v.speed/20,1),vx=cx+Math.sin(rd)*spd*35,vy=cy-Math.cos(rd)*spd*35;zx.setLineDash([2,3]);zx.beginPath();zx.moveTo(cx,cy);zx.lineTo(vx,vy);zx.strokeStyle=sc.mc+"99";zx.lineWidth=1;zx.stroke();zx.setLineDash([]);}
}

// ===== PANEL MODE FUNCTIONS =====
function setShipMode(){
  const hide=["det-hd","det-body","det-ap-hd","ap-tab-nav","ap-body","det-port-hd","port-tab-nav","port-body"];
  const show=["det-sh-hd","sh-tab-nav","sh-body"];
  hide.forEach(id=>document.getElementById(id).style.display="none");
  show.forEach(id=>document.getElementById(id).style.display="");
  document.getElementById("sh-tab-nav").style.display="flex";
  document.getElementById("sh-body").style.display="block";
  document.getElementById("sh-body").style.position="relative";
  document.getElementById("sh-body").style.overflow="hidden";
  document.getElementById("sh-body").style.flex="1";
}
function setPortMode(){
  const hide=["det-hd","det-body","det-ap-hd","ap-tab-nav","ap-body","det-sh-hd","sh-tab-nav","sh-body"];
  const show=["det-port-hd","port-tab-nav","port-body"];
  hide.forEach(id=>document.getElementById(id).style.display="none");
  show.forEach(id=>document.getElementById(id).style.display="");
  document.getElementById("port-tab-nav").style.display="flex";
  document.getElementById("port-body").style.display="block";
  document.getElementById("port-body").style.position="relative";
  document.getElementById("port-body").style.overflow="hidden";
  document.getElementById("port-body").style.flex="1";
}

// ===== SHIP TAB SWITCH =====
function switchShTab(tab){
  activeShTab=tab;
  document.querySelectorAll(".sh-tab-btn").forEach(b=>b.classList.toggle("on",b.dataset.shtab===tab));
  document.querySelectorAll(".sh-pane").forEach(p=>p.classList.toggle("on",p.id==="shtab-"+tab));
  if(tab==="nav"&&selShip)drawShipZoom();
}
document.getElementById("sh-tab-nav").addEventListener("click",e=>{const btn=e.target.closest(".sh-tab-btn");if(!btn)return;switchShTab(btn.dataset.shtab);});
document.getElementById("port-tab-nav").addEventListener("click",e=>{const btn=e.target.closest(".port-tab-btn");if(!btn)return;switchPortTab(btn.dataset.ptab);});
document.getElementById("det-sh-cls").addEventListener("click",()=>{selShip=null;closeDet();setFlightMode();document.getElementById("det-empty").style.display="flex";document.getElementById("det-flight").style.display="none";document.getElementById("tab-nav").style.display="none";drawMap();});
document.getElementById("det-port-cls").addEventListener("click",()=>{selPort=null;closeDet();setFlightMode();document.getElementById("det-empty").style.display="flex";document.getElementById("det-flight").style.display="none";document.getElementById("tab-nav").style.display="none";drawMap();});

function switchPortTab(tab){
  activePortTab=tab;
  document.querySelectorAll(".port-tab-btn").forEach(b=>b.classList.toggle("on",b.dataset.ptab===tab));
  document.querySelectorAll(".port-pane").forEach(p=>p.classList.toggle("on",p.id==="porttab-"+tab));
}

// ===== SHIP AI =====
function togShipAI(){
  shipAiOpen=!shipAiOpen;
  document.getElementById("sh-ai-bd").style.display=shipAiOpen?"block":"none";
  document.getElementById("sh-ai-arr").textContent=shipAiOpen?"Masquer ^":"Lancer v";
  if(shipAiOpen&&selShip&&selShip.mmsi!==lastShAI)loadShipAI(selShip);
}
async function loadShipAI(v){
  const sc=SC[v.type]||SC.unknown;lastShAI=v.mmsi;
  const ld=document.getElementById("sh-ai-ld"),sm=document.getElementById("sh-ai-sum"),tb=document.getElementById("sh-ai-tbl");
  ld.style.display="flex";sm.textContent="";tb.innerHTML="";
  const prompt=`Expert OSINT maritime. Navire: ${v.name} (MMSI: ${v.mmsi}, IMO: ${v.imo||"N/A"}), type: ${sc.lbl}, pavillon: ${v.flag||"N/A"}, longueur: ${v.length}m, destination: ${v.destination||"N/A"}, vitesse: ${v.speed?.toFixed(1)} kn, cap: ${Math.round(v.heading||0)}deg. Utilise la recherche web pour trouver des infos sur ce navire.
Réponds UNIQUEMENT en JSON valide sans markdown:
{"description":"description complète du navire et de son opérateur","role_economique":"rôle et importance économique","historique":"historique probable du navire et de ses routes","cargaison_probable":"estimation de la cargaison selon type et route","analyse_trajet":"analyse du trajet actuel et destination","importance_strategique":"importance stratégique ou économique","armateur":"nom de l'armateur ou compagnie","construction":"année de construction estimée","valeur_estimee":"valeur commerciale estimée du navire et cargaison"}`;
  try{
    const res=await fetch("https://api.anthropic.com/v1/messages",{method:"POST",headers:aiHeaders(),body:JSON.stringify({model:AI_MODEL,max_tokens:1500,tools:[{type:"web_search_20250305",name:"web_search"}],messages:[{role:"user",content:prompt}]})});
    if(!res.ok)throw new Error("HTTP "+res.status);
    const d=await res.json();const txt=d.content.filter(b=>b.type==="text").map(b=>b.text).join("");
    let info={};try{const m=txt.match(/\{[\s\S]*\}/);if(m)info=JSON.parse(m[0]);}catch(e){sm.textContent=txt;ld.style.display="none";return;}
    sm.textContent=info.description||"";
    tb.innerHTML=[["Armateur / Compagnie",info.armateur],["Construction",info.construction],["Cargaison probable",info.cargaison_probable],["Valeur estimee",info.valeur_estimee],["Role economique",info.role_economique],["Importance strategique",info.importance_strategique]].filter(([,v])=>v).map(([k,v])=>`<tr><td>${k}</td><td>${v}</td></tr>`).join("");
    ld.style.display="none";
  }catch(e){ld.style.display="none";sm.textContent="Erreur IA: "+e.message;}
}

// ===== PORT AI =====
async function loadPortAI(p){
  document.getElementById("port-ld").style.display="flex";
  document.getElementById("port-ai-sum").textContent="";document.getElementById("port-ai-tbl").innerHTML="";
  const prompt=`Expert logistique portuaire. Analyse le port de ${p.name}, ${p.country}. Utilise la recherche web. Réponds UNIQUEMENT en JSON valide sans markdown:
{"summary":"résumé stratégique et économique du port en 3-4 phrases","principales_routes":"principales routes maritimes desservies","compagnies":"principales compagnies maritimes utilisant ce port","concurrents":"ports concurrents","projets_recents":"projets d'expansion ou actualités récentes","importance":"importance géopolitique et économique","trafic_detail":"détail du trafic par type de marchandise"}`;
  try{
    const res=await fetch("https://api.anthropic.com/v1/messages",{method:"POST",headers:aiHeaders(),body:JSON.stringify({model:AI_MODEL,max_tokens:1200,tools:[{type:"web_search_20250305",name:"web_search"}],messages:[{role:"user",content:prompt}]})});
    if(!res.ok)throw new Error("HTTP "+res.status);
    const d=await res.json();const txt=d.content.filter(b=>b.type==="text").map(b=>b.text).join("");
    let info={};try{const m=txt.match(/\{[\s\S]*\}/);if(m)info=JSON.parse(m[0]);}catch(e){document.getElementById("port-ai-sum").textContent=txt;document.getElementById("port-ld").style.display="none";return;}
    document.getElementById("port-ai-sum").textContent=info.summary||"";
    document.getElementById("port-ai-tbl").innerHTML=[["Routes principales",info.principales_routes],["Compagnies",info.compagnies],["Concurrents",info.concurrents],["Trafic detail",info.trafic_detail],["Importance",info.importance],["Projets recents",info.projets_recents]].filter(([,v])=>v).map(([k,v])=>`<tr><td>${k}</td><td>${v}</td></tr>`).join("");
    document.getElementById("port-summary").textContent=info.summary||"-";
    document.getElementById("port-ld").style.display="none";
  }catch(e){document.getElementById("port-ai-sum").textContent="Erreur IA: "+e.message;document.getElementById("port-ld").style.display="none";}
}

// ===== SIDEBAR MODE =====
let sbMode="aerial",shipChip="all",shipQry="";
let allShowAir=true,allShowShip=true,allShowPort=true,allQry="";

function setSbMode(mode){
  sbMode=mode;
  // Button states
  document.getElementById("sb-btn-aer").className="sb-mode-btn aer"+(mode==="aerial"?" on":"");
  document.getElementById("sb-btn-mar").className="sb-mode-btn mar"+(mode==="maritime"?" on":"");
  document.getElementById("sb-btn-all").className="sb-mode-btn all"+(mode==="all"?" on":"");
  // Pane visibility
  document.getElementById("sb-aerial").style.display   = mode==="aerial"  ?"flex":"none";
  document.getElementById("sb-maritime").style.display = mode==="maritime" ?"flex":"none";
  document.getElementById("sb-all").style.display      = mode==="all"      ?"flex":"none";
  // Make sure map layers match
  if(mode==="all"){showShips=true;showPorts=true;showZones=true;drawMap();}
  if(mode==="all"){renderAllList();}
  if(mode==="maritime"){renderShipList();renderPortList();}
}

// ===== ALL CHIP FILTER =====
function setAllChip(type,btn){
  if(type==="air") allShowAir=!allShowAir;
  if(type==="ship") allShowShip=!allShowShip;
  if(type==="port") allShowPort=!allShowPort;
  btn.classList.toggle("on",
    type==="air"?allShowAir:type==="ship"?allShowShip:allShowPort
  );
  // sync map layer toggles
  if(type==="ship"){showShips=allShowShip;if(globeInited)toggleGlobeShips(showShips);}
  if(type==="port"){showPorts=allShowPort;if(globeInited)toggleGlobePorts(showPorts);}
  drawMap();
  renderAllList();
}

document.getElementById("all-input").addEventListener("input",e=>{allQry=e.target.value.toLowerCase();renderAllList();});

// ===== COMBINED LIST =====
function renderAllList(){
  const q=allQry;
  let items=[];

  // --- Flights ---
  if(allShowAir){
    const aList=filt.filter(f=>!q||f.callsign.toLowerCase().includes(q)||f.origin_country.toLowerCase().includes(q));
    aList.forEach(f=>{
      const t=TC[f.type]||TC.unknown,isSel=sel?.icao24===f.icao24;
      items.push({
        sort:f.type==="military"?0:f.type==="commercial"?1:2,
        html:`<div class="fl${isSel?" sel":""}" style="display:grid;grid-template-columns:3px 1fr 52px;min-height:36px;cursor:pointer;border-bottom:1px solid var(--b1);transition:background .07s" onclick="pick(all.find(x=>x.icao24==='${f.icao24}'))" onmouseover="this.style.background='var(--bg2)'" onmouseout="this.style.background=isSel?'rgba(255,255,255,.04)':''" >
          <div style="width:3px;background:${t.c}${isSel?"":"55"}"></div>
          <div style="padding:5px 9px;min-width:0">
            <div style="font-size:11px;font-weight:600;font-family:'JetBrains Mono',monospace;color:${isSel?t.c:"var(--t1)"};white-space:nowrap;overflow:hidden;text-overflow:ellipsis">
              <span style="font-size:8px;margin-right:3px;vertical-align:middle">✈</span>${f.callsign}
              <span style="font-size:9px;color:var(--t4);font-weight:400"> ${ccode(f.origin_country)}</span>
            </div>
            <div style="font-size:9px;color:var(--t3);margin-top:2px;display:flex;gap:4px">
              <span style="color:${t.c}">${t.lbl}</span><span>${f.origin_country}</span>
            </div>
          </div>
          <div style="padding:4px 7px 4px 0;text-align:right">
            <div style="font-size:10px;font-family:'JetBrains Mono',monospace;color:var(--t2)">${Math.round(f.baro_altitude||0)}m</div>
            <div style="font-size:9px;color:var(--t4)">${ago(f.last_contact)}</div>
          </div>
        </div>`
      });
    });
  }

  // --- Ships ---
  if(allShowShip){
    const sList=vessels.filter(v=>!q||v.name.toLowerCase().includes(q)||v.mmsi.includes(q)||(v.destination||"").toLowerCase().includes(q));
    sList.forEach(v=>{
      const sc=SC[v.type]||SC.unknown,isSel=selShip?.mmsi===v.mmsi;
      const hasAno=v.anomalies?.some(a=>a.type==="crit"||a.type==="warn");
      items.push({
        sort:v.type==="military"?0:3,
        html:`<div class="sh-item${isSel?" sel":""}" style="display:grid;grid-template-columns:3px 1fr 54px;min-height:36px;cursor:pointer;border-bottom:1px solid var(--b1);transition:background .07s" onclick="pickShip(vessels.find(x=>x.mmsi==='${v.mmsi}'))">
          <div style="width:3px;background:${sc.c}${isSel?"":"66"}"></div>
          <div style="padding:5px 9px;min-width:0">
            <div style="font-size:11px;font-weight:600;font-family:'JetBrains Mono',monospace;color:${isSel?sc.c:"var(--t1)"};white-space:nowrap;overflow:hidden;text-overflow:ellipsis">
              ${hasAno?'<span style="color:var(--red);margin-right:3px">!</span>':''}<span style="font-size:8px;margin-right:3px;vertical-align:middle">⛵</span>${v.name}
            </div>
            <div style="font-size:9px;color:var(--t3);margin-top:2px;display:flex;gap:4px">
              <span style="color:${sc.c};font-weight:600">${sc.lbl}</span>
              <span style="color:var(--t4)">${v.flag||""}</span>
              ${v.destination?`<span>→ ${v.destination.slice(0,8)}</span>`:""}
            </div>
          </div>
          <div style="padding:4px 7px 4px 0;text-align:right">
            <div style="font-size:10px;font-family:'JetBrains Mono',monospace;color:var(--t2)">${v.speed?.toFixed(1)||"0"} kn</div>
            <div style="font-size:9px;color:var(--t4)">${v.length||"?"}m</div>
          </div>
        </div>`
      });
    });
  }

  // --- Ports ---
  if(allShowPort){
    const pList=PORTS.filter(p=>!q||p.name.toLowerCase().includes(q)||p.country.toLowerCase().includes(q));
    pList.forEach(p=>{
      const tc=PORT_TYPES[p.type]||"#d4a020",isSel=selPort?.id===p.id;
      items.push({
        sort:10,
        html:`<div class="port-item${isSel?" sel":""}" style="display:grid;grid-template-columns:3px 1fr 50px;min-height:34px;cursor:pointer;border-bottom:1px solid var(--b1);transition:background .07s" onclick="pickPort(PORTS.find(x=>x.id==='${p.id}'))">
          <div style="width:3px;background:${tc}${isSel?"":"66"}"></div>
          <div style="padding:5px 9px;min-width:0">
            <div style="font-size:11px;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;color:${isSel?tc:"var(--t1)"}">
              <span style="font-size:8px;margin-right:3px;vertical-align:middle">⚓</span>${p.name}
            </div>
            <div style="font-size:9px;color:var(--t3);margin-top:2px">
              <span style="color:${tc};font-weight:600">${p.type}</span> <span style="color:var(--t4)">${p.country}</span>
            </div>
          </div>
          <div style="padding:4px 6px 4px 0;text-align:right">
            <div style="font-size:8px;color:var(--t4)">${p.cap.split("/")[0]}</div>
            <div style="font-size:8px;color:var(--t4)">${p.depth}</div>
          </div>
        </div>`
      });
    });
  }

  // Update counts
  document.getElementById("all-ct-air").textContent=allShowAir?filt.length:0;
  document.getElementById("all-ct-ship").textContent=allShowShip?vessels.length:0;
  document.getElementById("all-ct-port").textContent=allShowPort?PORTS.length:0;
  document.getElementById("sb-all-ct").textContent=items.length;

  // Sort: military first, then by type
  items.sort((a,b)=>a.sort-b.sort);

  const el=document.getElementById("all-list");
  if(!items.length){
    el.innerHTML=`<div style="padding:14px;font-size:10px;color:var(--t4);text-align:center">Aucun element a afficher.<br>Activez les couches ci-dessus.</div>`;
    return;
  }
  el.innerHTML=items.map(i=>i.html).join("");
}

// ===== SHIP LIST =====
function setShipChip(btn,type){
  document.querySelectorAll(".mar-chips .chip").forEach(b=>b.classList.remove("on"));
  btn.classList.add("on");shipChip=type;renderShipList();
}
document.getElementById("mar-input").addEventListener("input",e=>{shipQry=e.target.value.toLowerCase();renderShipList();renderPortList();});

function renderShipList(){
  const ql=shipQry;
  let list=vessels.filter(v=>{
    if(shipChip!=="all"&&v.type!==shipChip)return false;
    return !ql||v.name.toLowerCase().includes(ql)||v.mmsi.includes(ql)||(v.destination||"").toLowerCase().includes(ql);
  });
  document.getElementById("sh-list-ct").textContent=list.length+" navires";
  document.getElementById("sb-aer-ct").textContent=filt.length;
  document.getElementById("sb-mar-ct").textContent=vessels.length;
  const anoCt=vessels.filter(v=>v.anomalies?.some(a=>a.type==="crit"||a.type==="warn")).length;
  document.getElementById("ano-ct").textContent=anoCt+" alerte"+(anoCt>1?"s":"");
  const el=document.getElementById("sh-list");
  if(!list.length){el.innerHTML=`<div style="padding:10px;font-size:10px;color:var(--t4);text-align:center">Aucun navire dans cette region</div>`;return;}
  el.innerHTML=list.map(v=>{
    const sc=SC[v.type]||SC.unknown,isSel=selShip?.mmsi===v.mmsi;
    const hasAno=v.anomalies?.some(a=>a.type==="crit"||a.type==="warn");
    return`<div class="sh-item${isSel?" sel":""}" data-mmsi="${v.mmsi}" onclick="pickShip(vessels.find(x=>x.mmsi==='${v.mmsi}'))">
      <div class="sh-bar" style="background:${sc.c}${isSel?"":"66"}"></div>
      <div class="sh-b">
        <div class="sh-nm" style="color:${isSel?sc.c:"var(--t1)"}">
          ${hasAno?`<span class="ano-dot" style="display:inline-block;margin-right:3px"></span>`:""}${v.name}
        </div>
        <div class="sh-meta">
          <span style="color:${sc.c};font-size:9px;font-weight:600">${sc.lbl}</span>
          <span style="color:var(--t4)">${v.flag||"??"}</span>
          ${v.destination?`<span style="color:var(--t3)">→ ${v.destination.slice(0,8)}</span>`:""}
        </div>
      </div>
      <div class="sh-r">
        <div class="sh-spd">${v.speed?.toFixed(1)||"0"} kn</div>
        <div class="sh-len">${v.length||"?"}m</div>
      </div>
    </div>`;
  }).join("");
}

function renderPortList(){
  const ql=shipQry;
  const list=PORTS.filter(p=>!ql||p.name.toLowerCase().includes(ql)||p.country.toLowerCase().includes(ql)||p.type.toLowerCase().includes(ql));
  document.getElementById("port-list-ct").textContent=list.length+" ports";
  const el=document.getElementById("port-list");
  el.innerHTML=list.map(p=>{
    const tc=PORT_TYPES[p.type]||"#d4a020",isSel=selPort?.id===p.id;
    return`<div class="port-item${isSel?" sel":""}" onclick="pickPort(PORTS.find(x=>x.id==='${p.id}'))">
      <div class="port-bar" style="background:${tc}${isSel?"":"66"}"></div>
      <div class="port-b">
        <div class="port-nm-s" style="color:${isSel?tc:"var(--t1)"}">${p.name}</div>
        <div class="port-meta"><span style="color:${tc};font-weight:600;font-size:9px">${p.type}</span> <span style="color:var(--t4)">${p.country}</span></div>
      </div>
      <div class="port-r">
        <div class="port-cap-s" style="font-size:8px;color:var(--t4)">${p.cap.split("/")[0]}</div>
        <div style="font-size:8px;color:var(--t4)">${p.depth}</div>
      </div>
    </div>`;
  }).join("");
}

// ===== SIDEBAR MODE =====
function updateMaritimeStats(){
  document.getElementById("sv-ships").textContent=vessels.length;
  document.getElementById("sv-ports").textContent=PORTS.length;
  document.getElementById("sb-aer-ct").textContent=filt.length;
  document.getElementById("sb-mar-ct").textContent=vessels.length;
  if(document.getElementById("ais-src-lbl"))document.getElementById("ais-src-lbl").textContent=aisConnected?"AIS Live":"Demo";
  const anoCt=vessels.filter(v=>v.anomalies?.some(a=>a.type==="crit"||a.type==="warn")).length;
  if(document.getElementById("ano-ct"))document.getElementById("ano-ct").textContent=anoCt+" alerte"+(anoCt>1?"s":"");
  if(globeInited)document.getElementById("gc-ships").textContent=vessels.length;
  if(sbMode==="maritime"){renderShipList();renderPortList();}
  if(sbMode==="all"){renderAllList();}
}

// ===== LAYER TOGGLES =====
function toggleShowShips(){showShips=!showShips;document.getElementById("btn-ships-tog").classList.toggle("on",showShips);drawMap();if(globeInited)toggleGlobeShips(showShips);}
function toggleShowPorts(){showPorts=!showPorts;document.getElementById("btn-ports-tog").classList.toggle("on",showPorts);drawMap();if(globeInited)toggleGlobePorts(showPorts);}
function toggleShowZones(){showZones=!showZones;document.getElementById("btn-zones-tog").classList.toggle("on",showZones);drawMap();if(globeInited)toggleGlobeZones(showZones);}
function toggleGlobeShips(v){Object.values(gShipEntities).forEach(e=>{if(e&&!e.isDestroyed)e.show=v;});}
function toggleGlobePorts(v){Object.values(gPortEntities).forEach(e=>{if(e&&!e.isDestroyed)e.show=v;});}
function toggleGlobeZones(v){gZoneEntities.forEach(e=>{if(e&&!e.isDestroyed)e.show=v;});}

// ===== GLOBE: SHIP ENTITIES =====
function makeShipCanvas(color,sz=26){
  const c=document.createElement("canvas");c.width=c.height=sz;
  const x=c.getContext("2d"),h=sz/2,sl=sz*.4,sw=sz*.15;
  x.fillStyle=color;x.shadowColor=color;x.shadowBlur=6;
  x.beginPath();x.moveTo(h,3);x.lineTo(h+sw,h-sl*.5);x.lineTo(h+sw,h+sl*.6);x.lineTo(h-sw,h+sl*.6);x.lineTo(h-sw,h-sl*.5);x.closePath();x.fill();
  return c;
}
function makePortCanvas(color,sz=18){
  const c=document.createElement("canvas");c.width=c.height=sz;
  const x=c.getContext("2d"),h=sz/2,s=sz*.4;
  x.fillStyle=color+"66";x.strokeStyle=color;x.lineWidth=1.5;
  x.save();x.translate(h,h);x.rotate(Math.PI/4);x.fillRect(-s,-s,s*2,s*2);x.strokeRect(-s,-s,s*2,s*2);x.restore();
  return c;
}

function updateGlobeShips(){
  if(!gViewer)return;
  const existing=new Set(Object.keys(gShipEntities));
  vessels.forEach(v=>{
    const pos=Cesium.Cartesian3.fromDegrees(v.longitude,v.latitude,0);
    const sc=SC[v.type]||SC.unknown,isSel=selShip?.mmsi===v.mmsi;
    if(gShipEntities[v.mmsi]){
      const e=gShipEntities[v.mmsi];
      e.position=new Cesium.ConstantPositionProperty(pos);
      e.billboard.rotation=-Cesium.Math.toRadians(v.heading||0);
      e.billboard.scale=isSel?1.8:1;e.label.show=isSel;
      e._vd=v;existing.delete(v.mmsi);
    }else{
      const e=gViewer.entities.add({
        id:"gs_"+v.mmsi,position:pos,
        billboard:{image:makeShipCanvas(sc.mc,isSel?30:22),scale:isSel?1.8:1,rotation:-Cesium.Math.toRadians(v.heading||0),alignedAxis:Cesium.Cartesian3.UNIT_Z,verticalOrigin:Cesium.VerticalOrigin.CENTER,disableDepthTestDistance:Number.POSITIVE_INFINITY},
        label:{text:v.name,font:"bold 9px JetBrains Mono,monospace",fillColor:Cesium.Color.fromCssColorString(sc.mc),outlineColor:Cesium.Color.BLACK,outlineWidth:2,style:Cesium.LabelStyle.FILL_AND_OUTLINE,pixelOffset:new Cesium.Cartesian2(14,0),disableDepthTestDistance:Number.POSITIVE_INFINITY,show:isSel},
        show:showShips&&document.getElementById("tog-globe-ships").checked,
      });
      e._vd=v;gShipEntities[v.mmsi]=e;
    }
  });
  existing.forEach(mmsi=>{if(gShipEntities[mmsi]){gViewer.entities.remove(gShipEntities[mmsi]);delete gShipEntities[mmsi];}});
  document.getElementById("gc-ships").textContent=vessels.length;
}

function addGlobePorts(){
  if(!gViewer)return;
  PORTS.forEach(p=>{
    const pos=Cesium.Cartesian3.fromDegrees(p.lon,p.lat,0);
    const tc=PORT_TYPES[p.type]||"#d4a020";
    const e=gViewer.entities.add({
      id:"gp_"+p.id,position:pos,
      billboard:{image:makePortCanvas(tc,18),scale:1,verticalOrigin:Cesium.VerticalOrigin.CENTER,disableDepthTestDistance:Number.POSITIVE_INFINITY,scaleByDistance:new Cesium.NearFarScalar(200000,1.2,12000000,0.3)},
      label:{text:p.name,font:"bold 8px JetBrains Mono,monospace",fillColor:Cesium.Color.fromCssColorString(tc),outlineColor:Cesium.Color.BLACK,outlineWidth:2,style:Cesium.LabelStyle.FILL_AND_OUTLINE,pixelOffset:new Cesium.Cartesian2(12,0),disableDepthTestDistance:Number.POSITIVE_INFINITY,scaleByDistance:new Cesium.NearFarScalar(200000,1,10000000,0),translucencyByDistance:new Cesium.NearFarScalar(500000,1,8000000,0)},
      show:showPorts,
    });
    e._pd=p;gPortEntities[p.id]=e;
  });
  document.getElementById("gc-ports").textContent=PORTS.length;
}

function addGlobeStrategicZones(){
  if(!gViewer)return;
  STRAT_ZONES.forEach(z=>{
    const c=z.border;
    const e=gViewer.entities.add({
      rectangle:{coordinates:Cesium.Rectangle.fromDegrees(z.minLon,z.minLat,z.maxLon,z.maxLat),material:Cesium.Color.fromCssColorString(z.color.replace(/rgba\(([^)]+)\)/,(_,v)=>{const[r,g,b,a]=v.split(",").map(Number);return`rgba(${r},${g},${b},${a})`})||"rgba(200,50,50,0.1)"),outline:true,outlineColor:Cesium.Color.fromCssColorString(c||"rgba(200,50,50,0.5)"),outlineWidth:2},
      label:{text:z.name,position:Cesium.Cartesian3.fromDegrees((z.minLon+z.maxLon)/2,(z.minLat+z.maxLat)/2,1000),font:"bold 10px JetBrains Mono,monospace",fillColor:Cesium.Color.fromCssColorString(c||"rgba(200,50,50,.8)"),outlineColor:Cesium.Color.BLACK,outlineWidth:2,style:Cesium.LabelStyle.FILL_AND_OUTLINE,disableDepthTestDistance:Number.POSITIVE_INFINITY,scaleByDistance:new Cesium.NearFarScalar(500000,1,6000000,0)},
      show:showZones,
    });
    gZoneEntities.push(e);
  });
}

// ===== INIT MARITIME =====
function initMaritime(){
  loadVessels();
  updateMaritimeStats();
  // Refresh vessels every 30s (or live via AISStream WebSocket)
  vesselInt=setInterval(loadVessels, 30000);
  // Smooth demo movement tick every 4s (ships move slowly)
  setInterval(()=>{
    if(aisConnected) return; // live feed handles positions
    const b=REGIONS[region];
    vessels.forEach(v=>{
      const spd=(v.speed||8)/111000*4;
      const r=((v.heading||v.cog||0)*Math.PI)/180;
      v.latitude =Math.max(b.minLat+.1, Math.min(b.maxLat-.1, v.latitude +Math.cos(r)*spd));
      v.longitude=Math.max(b.minLon+.1, Math.min(b.maxLon-.1, v.longitude+Math.sin(r)*spd));
      // Slowly add to trail
      if(!shipTrails[v.mmsi]) shipTrails[v.mmsi]=[];
      const last=shipTrails[v.mmsi].slice(-1)[0];
      if(!last||Math.hypot(v.latitude-last.lat,v.longitude-last.lon)>0.005){
        shipTrails[v.mmsi].push({lat:v.latitude,lon:v.longitude});
        if(shipTrails[v.mmsi].length>30) shipTrails[v.mmsi].shift();
      }
      // Update anomalies live
      v.anomalies=detectAnomalies(v);
    });
    // Redraw only if in 2D view
    if(currentView==="2d") drawMap();
    // Update globe ships
    if(globeInited) updateGlobeShips();
    // Refresh selected ship panel
    if(selShip){
      const updated=vessels.find(v=>v.mmsi===selShip.mmsi);
      if(updated){
        selShip=updated;
        if(activeShTab==="nav") drawShipZoom();
        document.getElementById("sh-spd").textContent=fKts(updated.speed);
        document.getElementById("sh-hdg").textContent=fDeg2(updated.heading);
      }
    }
    if(sbMode==="maritime") renderShipList();
    if(sbMode==="all") renderAllList();
  }, 4000);
}

// ============================================================
// ===== INIT =====
// ============================================================
// ===== MOTEUR CARTOGRAPHIQUE 2D (MapLibre GL) =====
// Les icones/trajectoires restent dessinees par le canvas overlay,
// mais projetees par MapLibre (zoom, pan, rotation, tuiles reelles).
// ============================================================
const BASEMAPS={
  dark:{tiles:["https://a.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png","https://b.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png"],attr:"&copy; OpenStreetMap &copy; CARTO",max:19},
  light:{tiles:["https://a.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png","https://b.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png"],attr:"&copy; OpenStreetMap &copy; CARTO",max:19},
  osm:{tiles:["https://tile.openstreetmap.org/{z}/{x}/{y}.png"],attr:"&copy; OpenStreetMap contributors",max:19},
  sat:{tiles:["https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}"],attr:"Imagery &copy; Esri, Maxar, Earthstar Geographics",max:19}
};
function queueDraw(){if(drawQ)return;drawQ=true;requestAnimationFrame(()=>{drawQ=false;drawMap();});}
function initGL(){
  if(gl||typeof maplibregl==="undefined"){if(typeof maplibregl==="undefined")console.warn("MapLibre indisponible: rendu canvas de secours");return;}
  const sources={},layers=[{id:"bg",type:"background",paint:{"background-color":"#060606"}}];
  Object.keys(BASEMAPS).forEach(k=>{
    sources["bm-"+k]={type:"raster",tiles:BASEMAPS[k].tiles,tileSize:256,maxzoom:BASEMAPS[k].max,attribution:BASEMAPS[k].attr};
    layers.push({id:"bm-"+k,type:"raster",source:"bm-"+k,layout:{visibility:k===currentBM?"visible":"none"}});
  });
  try{
    gl=new maplibregl.Map({container:"map-gl",style:{version:8,sources,layers},center:[10,48],zoom:3,maxPitch:60,attributionControl:{compact:true}});
  }catch(e){console.warn("MapLibre init:",e);gl=null;return;}
  document.getElementById("mw").classList.add("gl");
  gl.addControl(new maplibregl.NavigationControl({visualizePitch:true}),"top-right");
  gl.addControl(new maplibregl.ScaleControl({unit:"metric"}),"bottom-right");
  gl.on("move",queueDraw);
  gl.on("resize",queueDraw);
  // Le viewport devient la zone interrogee (API avions / AIS / filtres)
  gl.on("moveend",()=>{
    const bd=gl.getBounds(),R=REGIONS[region];
    R.minLat=Math.max(-85,bd.getSouth());R.maxLat=Math.min(85,bd.getNorth());
    R.minLon=Math.max(-180,bd.getWest());R.maxLon=Math.min(180,bd.getEast());
    drawMap();
  });
  const fwd=(type,oe)=>{if(!oe)return;cvs.dispatchEvent(new MouseEvent(type,{clientX:oe.clientX,clientY:oe.clientY}));gl.getCanvas().style.cursor=cvs.style.cursor==="pointer"?"pointer":"";};
  gl.on("mousemove",e=>fwd("mousemove",e.originalEvent));
  gl.on("click",e=>fwd("click",e.originalEvent));
  gl.getCanvas().addEventListener("mouseleave",()=>cvs.dispatchEvent(new MouseEvent("mouseleave")));
  fitRegion(0);
}
function fitRegion(dur){
  if(!gl)return;
  const b=REGION_PRESETS[region];
  gl.fitBounds([[b.minLon,Math.max(-80,b.minLat)],[b.maxLon,Math.min(80,b.maxLat)]],{padding:20,duration:dur==null?900:dur});
}
function setBasemap(k,btn){
  currentBM=k;
  document.querySelectorAll(".bm").forEach(b=>b.classList.toggle("on",b===btn));
  if(!gl)return;
  Object.keys(BASEMAPS).forEach(id=>gl.setLayoutProperty("bm-"+id,"visibility",id===k?"visible":"none"));
}
function centerOn(lat,lon){if(gl&&currentView==="2d"&&lat!=null&&lon!=null)gl.easeTo({center:[lon,lat],duration:700});}

showPanelEmpty();
if(CFG.aisStream){const ak=document.getElementById('ais-key');if(ak)ak.value=CFG.aisStream;}
if(CFG.cesium){const it=document.getElementById('ion-token');if(it)it.value=CFG.cesium;}
setSbMode('aerial');
loadFlights();
refInt=setInterval(loadFlights, 15000);
resizeMap();
initGL();
resizeMap();
initMaritime();
if(CFG.aisStream)setTimeout(connectAISStream,1500);
</script>
</body>
</html>
