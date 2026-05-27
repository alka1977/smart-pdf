import { useState, useEffect, useRef, useCallback } from "react";

// ─── Library loading (starts the moment this module loads) ───────────────────
const LIB = { pdfLib: null, pdfjs: null };

const pdfLibReady = new Promise((resolve, reject) => {
  const s = document.createElement("script");
  s.src = "https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js";
  s.onload = () => { LIB.pdfLib = window.PDFLib; resolve(); };
  s.onerror = () => reject(new Error("pdf-lib failed to load"));
  document.head.appendChild(s);
});

const pdfjsReady = new Promise((resolve, reject) => {
  const s = document.createElement("script");
  s.src = "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js";
  s.onload = () => {
    window.pdfjsLib.GlobalWorkerOptions.workerSrc =
      "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";
    LIB.pdfjs = window.pdfjsLib;
    resolve();
  };
  s.onerror = () => reject(new Error("pdf.js failed to load"));
  document.head.appendChild(s);
});

// ─── Tool definitions ─────────────────────────────────────────────────────────
const TOOLS = [
  { id: "jpg-to-pdf",  name: "JPG → PDF",      icon: "🖼",  color: "#F97316", desc: "Convert JPG / PNG images to a PDF",        accept: "image/*",                multi: true,  works: true  },
  { id: "pdf-to-jpg",  name: "PDF → JPG",       icon: "📷",  color: "#EF4444", desc: "Export every PDF page as a JPG image",    accept: ".pdf",                   multi: false, works: true  },
  { id: "merge",       name: "Merge PDF",        icon: "⊕",   color: "#3B82F6", desc: "Combine multiple PDFs into one file",     accept: ".pdf",                   multi: true,  works: true  },
  { id: "split",       name: "Split PDF",        icon: "✂",   color: "#8B5CF6", desc: "Split PDF into individual pages",         accept: ".pdf",                   multi: false, works: true  },
  { id: "compress",    name: "Compress PDF",     icon: "⬇",   color: "#10B981", desc: "Reduce file size with object streams",    accept: ".pdf",                   multi: false, works: true  },
  { id: "rotate",      name: "Rotate PDF",       icon: "↻",   color: "#06B6D4", desc: "Rotate every page by 90 / 180 / 270 °",  accept: ".pdf",                   multi: false, works: true  },
  { id: "watermark",   name: "Watermark",        icon: "◈",   color: "#7C3AED", desc: "Stamp diagonal text on every page",       accept: ".pdf",                   multi: false, works: true  },
  { id: "page-num",    name: "Page Numbers",     icon: "#",   color: "#059669", desc: "Add page numbers to footer",              accept: ".pdf",                   multi: false, works: true  },
  { id: "unlock",      name: "Unlock PDF",       icon: "🔓",  color: "#F59E0B", desc: "Strip encryption flag from a PDF",        accept: ".pdf",                   multi: false, works: true  },
  { id: "repair",      name: "Repair PDF",       icon: "🔧",  color: "#6B7280", desc: "Re-save and normalise a broken PDF",      accept: ".pdf",                   multi: false, works: true  },
  { id: "pdf-to-word", name: "PDF → Word",       icon: "W",   color: "#2563EB", desc: "Server-side conversion (demo note)",      accept: ".pdf",                   multi: false, works: false },
  { id: "word-to-pdf", name: "Word → PDF",       icon: "↗",   color: "#1D4ED8", desc: "Server-side conversion (demo note)",      accept: ".doc,.docx",             multi: false, works: false },
  { id: "ocr",         name: "OCR / Extract",    icon: "🔍",  color: "#0891B2", desc: "AI text extraction (demo note)",          accept: ".pdf,image/*",           multi: false, works: false },
  { id: "sign",        name: "Sign PDF",         icon: "✍",  color: "#065F46", desc: "Digital signatures (demo note)",          accept: ".pdf",                   multi: false, works: false },
  { id: "protect",     name: "Protect PDF",      icon: "🔒",  color: "#DC2626", desc: "Password-protect a PDF (demo note)",     accept: ".pdf",                   multi: false, works: false },
  { id: "excel-pdf",   name: "Excel → PDF",      icon: "X",   color: "#047857", desc: "Server-side conversion (demo note)",      accept: ".xls,.xlsx",             multi: false, works: false },
  { id: "ppt-pdf",     name: "PPT → PDF",        icon: "P",   color: "#B45309", desc: "Server-side conversion (demo note)",      accept: ".ppt,.pptx",             multi: false, works: false },
  { id: "html-pdf",    name: "HTML → PDF",       icon: "</>", color: "#6D28D9", desc: "Server-side conversion (demo note)",      accept: ".html,.htm",             multi: false, works: false },
  { id: "edit",        name: "Edit PDF",         icon: "✏",  color: "#4F46E5", desc: "Online editor (demo note)",               accept: ".pdf",                   multi: false, works: false },
  { id: "organize",    name: "Organize Pages",   icon: "⊞",  color: "#92400E", desc: "Drag-reorder pages (demo note)",          accept: ".pdf",                   multi: false, works: false },
];

// ─── PDF processors ───────────────────────────────────────────────────────────
async function doJpgToPdf(files, prog) {
  await pdfLibReady;
  const { PDFDocument } = LIB.pdfLib;
  const doc = await PDFDocument.create();
  for (let i = 0; i < files.length; i++) {
    prog(Math.round(((i + 1) / files.length) * 85));
    const ab  = await files[i].arrayBuffer();
    const img = files[i].type === "image/png"
      ? await doc.embedPng(ab).catch(() => doc.embedJpg(ab))
      : await doc.embedJpg(ab);
    const { width: iw, height: ih } = img;
    const pw = 595.28, ph = 841.89;
    const s  = Math.min(pw / iw, ph / ih, 1);
    const pg = doc.addPage([pw, ph]);
    pg.drawImage(img, { x: (pw - iw * s) / 2, y: (ph - ih * s) / 2, width: iw * s, height: ih * s });
  }
  prog(95);
  const bytes = await doc.save();
  prog(100);
  return [{ name: "converted.pdf", bytes, type: "application/pdf" }];
}

async function doPdfToJpg(file, prog) {
  await pdfjsReady;
  const ab  = await file.arrayBuffer();
  const pdf = await LIB.pdfjs.getDocument({ data: ab }).promise;
  const out = [];
  for (let i = 1; i <= pdf.numPages; i++) {
    prog(Math.round((i / pdf.numPages) * 92));
    const page   = await pdf.getPage(i);
    const vp     = page.getViewport({ scale: 2 });
    const canvas = Object.assign(document.createElement("canvas"), { width: vp.width, height: vp.height });
    await page.render({ canvasContext: canvas.getContext("2d"), viewport: vp }).promise;
    out.push({ name: `page_${i}.jpg`, dataUrl: canvas.toDataURL("image/jpeg", 0.92), isImage: true });
  }
  prog(100);
  return out;
}

async function doMerge(files, prog) {
  await pdfLibReady;
  const { PDFDocument } = LIB.pdfLib;
  const out = await PDFDocument.create();
  for (let i = 0; i < files.length; i++) {
    prog(Math.round(((i + 1) / files.length) * 85));
    const src = await PDFDocument.load(await files[i].arrayBuffer(), { ignoreEncryption: true });
    const pgs = await out.copyPages(src, src.getPageIndices());
    pgs.forEach(p => out.addPage(p));
  }
  prog(95);
  const bytes = await out.save();
  prog(100);
  return [{ name: "merged.pdf", bytes, type: "application/pdf", note: `${files.length} files merged` }];
}

async function doSplit(file, rangeStr, prog) {
  await pdfLibReady;
  const { PDFDocument } = LIB.pdfLib;
  const src  = await PDFDocument.load(await file.arrayBuffer(), { ignoreEncryption: true });
  const tot  = src.getPageCount();
  let idxs   = Array.from({ length: tot }, (_, i) => i);
  if (rangeStr.trim()) {
    idxs = [];
    rangeStr.split(",").forEach(part => {
      part = part.trim();
      if (part.includes("-")) {
        const [a, b] = part.split("-").map(n => Math.max(0, parseInt(n) - 1));
        for (let i = a; i <= Math.min(b, tot - 1); i++) idxs.push(i);
      } else { const n = parseInt(part) - 1; if (n >= 0 && n < tot) idxs.push(n); }
    });
  }
  const out = [];
  for (let i = 0; i < idxs.length; i++) {
    prog(Math.round(((i + 1) / idxs.length) * 88));
    const d = await PDFDocument.create();
    const [pg] = await d.copyPages(src, [idxs[i]]);
    d.addPage(pg);
    out.push({ name: `page_${idxs[i] + 1}.pdf`, bytes: await d.save(), type: "application/pdf" });
  }
  prog(100);
  return out;
}

async function doCompress(file, prog) {
  await pdfLibReady;
  const { PDFDocument } = LIB.pdfLib;
  const ab  = await file.arrayBuffer();
  prog(30);
  const pdf = await PDFDocument.load(ab, { ignoreEncryption: true });
  prog(65);
  const bytes = await pdf.save({ useObjectStreams: true });
  prog(100);
  const saved = Math.max(0, ab.byteLength - bytes.byteLength);
  return [{ name: "compressed.pdf", bytes, type: "application/pdf",
    note: `${fmt(ab.byteLength)} → ${fmt(bytes.byteLength)}  (saved ${fmt(saved)})` }];
}
const fmt = b => b > 1048576 ? (b / 1048576).toFixed(2) + " MB" : (b / 1024).toFixed(1) + " KB";

async function doRotate(file, angle, prog) {
  await pdfLibReady;
  const { PDFDocument, degrees } = LIB.pdfLib;
  const pdf = await PDFDocument.load(await file.arrayBuffer(), { ignoreEncryption: true });
  const pgs = pdf.getPages();
  pgs.forEach((pg, i) => { prog(Math.round(((i + 1) / pgs.length) * 80)); pg.setRotation(degrees(pg.getRotation().angle + angle)); });
  prog(100);
  return [{ name: "rotated.pdf", bytes: await pdf.save(), type: "application/pdf" }];
}

async function doWatermark(file, text, opacity, prog) {
  await pdfLibReady;
  const { PDFDocument, rgb, degrees, StandardFonts } = LIB.pdfLib;
  const pdf  = await PDFDocument.load(await file.arrayBuffer(), { ignoreEncryption: true });
  const font = await pdf.embedFont(StandardFonts.HelveticaBold);
  const pgs  = pdf.getPages();
  for (let i = 0; i < pgs.length; i++) {
    prog(Math.round(((i + 1) / pgs.length) * 80));
    const { width: w, height: h } = pgs[i].getSize();
    const sz = Math.min(w, h) * 0.10;
    const tw = font.widthOfTextAtSize(text, sz);
    pgs[i].drawText(text, { x: (w - tw) / 2, y: h / 2 - sz / 2, size: sz, font, color: rgb(.5,.5,.5), rotate: degrees(45), opacity: Math.min(1, Math.max(0.05, parseFloat(opacity))) });
  }
  prog(100);
  return [{ name: "watermarked.pdf", bytes: await pdf.save(), type: "application/pdf" }];
}

async function doPageNumbers(file, prog) {
  await pdfLibReady;
  const { PDFDocument, rgb, StandardFonts } = LIB.pdfLib;
  const pdf  = await PDFDocument.load(await file.arrayBuffer(), { ignoreEncryption: true });
  const font = await pdf.embedFont(StandardFonts.Helvetica);
  const pgs  = pdf.getPages();
  pgs.forEach((pg, i) => {
    prog(Math.round(((i + 1) / pgs.length) * 82));
    const { width: w } = pg.getSize();
    const t = `${i + 1} / ${pgs.length}`;
    pg.drawText(t, { x: (w - font.widthOfTextAtSize(t, 11)) / 2, y: 16, size: 11, font, color: rgb(.4,.4,.4) });
  });
  prog(100);
  return [{ name: "numbered.pdf", bytes: await pdf.save(), type: "application/pdf" }];
}

async function doUnlock(file, prog) {
  await pdfLibReady;
  const { PDFDocument } = LIB.pdfLib;
  const pdf = await PDFDocument.load(await file.arrayBuffer(), { ignoreEncryption: true });
  prog(70);
  return [{ name: "unlocked.pdf", bytes: await pdf.save(), type: "application/pdf" }];
}

async function doRepair(file, prog) {
  await pdfLibReady;
  const { PDFDocument } = LIB.pdfLib;
  prog(30);
  const pdf = await PDFDocument.load(await file.arrayBuffer(), { ignoreEncryption: true });
  prog(70);
  return [{ name: "repaired.pdf", bytes: await pdf.save({ useObjectStreams: true }), type: "application/pdf" }];
}

// ─── Download helper ──────────────────────────────────────────────────────────
function downloadResult(r) {
  if (r.dataUrl) {
    const a = Object.assign(document.createElement("a"), { href: r.dataUrl, download: r.name });
    document.body.appendChild(a); a.click(); document.body.removeChild(a);
  } else {
    const url = URL.createObjectURL(new Blob([r.bytes], { type: r.type }));
    const a   = Object.assign(document.createElement("a"), { href: url, download: r.name });
    document.body.appendChild(a); a.click(); document.body.removeChild(a);
    setTimeout(() => URL.revokeObjectURL(url), 2000);
  }
}

// ─── Main Component ───────────────────────────────────────────────────────────
export default function App() {
  const [dark,    setDark]    = useState(false);
  const [libOk,   setLibOk]   = useState({ pdfLib: false, pdfjs: false });
  const [activeTool, setActiveTool] = useState(null);
  const [files,   setFiles]   = useState([]);
  const [busy,    setBusy]    = useState(false);
  const [prog,    setProg]    = useState(0);
  const [results, setResults] = useState([]);
  const [errMsg,  setErrMsg]  = useState(null);
  const [toast,   setToast]   = useState(null);
  const [opts,    setOpts]    = useState({ angle: 90, wmText: "CONFIDENTIAL", wmOpacity: 0.3, splitRange: "" });
  const [dropHero, setDropHero] = useState(false);
  const [dropModal, setDropModal] = useState(false);
  const [openFaq, setOpenFaq] = useState(null);
  const [billing, setBilling] = useState("monthly");
  const [hov,     setHov]     = useState(null);
  const modalInputRef = useRef();
  const heroInputRef  = useRef();

  // Track lib loading
  useEffect(() => {
    pdfLibReady.then(() => setLibOk(p => ({ ...p, pdfLib: true }))).catch(() => {});
    pdfjsReady .then(() => setLibOk(p => ({ ...p, pdfjs:  true }))).catch(() => {});
  }, []);

  const showToast = (msg, type = "ok") => {
    setToast({ msg, type });
    setTimeout(() => setToast(null), 3000);
  };

  // Open tool modal
  function openModal(tool) {
    setActiveTool(tool);
    setFiles([]); setResults([]); setErrMsg(null); setProg(0); setBusy(false);
  }

  function addFiles(list) {
    const arr = Array.from(list).filter(f => f.size > 0);
    if (!arr.length) return;
    setFiles(activeTool?.multi ? prev => [...prev, ...arr] : arr);
    setResults([]); setErrMsg(null);
    showToast(`📂 ${arr.length} file${arr.length > 1 ? "s" : ""} added`);
  }

  async function runTool() {
    if (!activeTool || !files.length) { setErrMsg("Please add at least one file."); return; }
    setBusy(true); setProg(5); setErrMsg(null); setResults([]);
    try {
      let out;
      switch (activeTool.id) {
        case "jpg-to-pdf": out = await doJpgToPdf(files, setProg); break;
        case "pdf-to-jpg": out = await doPdfToJpg(files[0], setProg); break;
        case "merge":      out = await doMerge(files, setProg); break;
        case "split":      out = await doSplit(files[0], opts.splitRange, setProg); break;
        case "compress":   out = await doCompress(files[0], setProg); break;
        case "rotate":     out = await doRotate(files[0], opts.angle, setProg); break;
        case "watermark":  out = await doWatermark(files[0], opts.wmText, opts.wmOpacity, setProg); break;
        case "page-num":   out = await doPageNumbers(files[0], setProg); break;
        case "unlock":     out = await doUnlock(files[0], setProg); break;
        case "repair":     out = await doRepair(files[0], setProg); break;
        default: throw new Error("This tool requires server-side processing and is not available in the browser demo.");
      }
      setResults(out);
      showToast(`✅ Done! ${out.length} file${out.length > 1 ? "s" : ""} ready to download.`);
    } catch (e) {
      setErrMsg(e.message || "Processing failed. Please try again.");
    } finally {
      setBusy(false);
    }
  }

  // ── styles ──────────────────────────────────────────────────────────────────
  const d  = dark;
  const bg = d ? "#0D1117" : "#F6F8FA";
  const cd = d ? "#161B22" : "#FFFFFF";
  const sf = d ? "#0D1117" : "#EFF6FF";
  const br = d ? "rgba(255,255,255,0.09)" : "rgba(0,0,0,0.09)";
  const tx = d ? "#E6EDF3" : "#0D1117";
  const ts = d ? "#8B949E" : "#57606A";
  const ac = "#2563EB";

  const libAllReady = libOk.pdfLib && libOk.pdfjs;
  const needsPdfjs  = activeTool?.id === "pdf-to-jpg";
  const canProcess  = activeTool?.works && files.length > 0 &&
    (needsPdfjs ? libOk.pdfjs : libOk.pdfLib) && !busy;

  return (
    <div style={{ fontFamily: "'Sora','DM Sans',sans-serif", background: bg, color: tx, minHeight: "100vh", overflowX: "hidden", transition: "background .3s,color .3s" }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Sora:wght@300;400;500;600;700;800&family=DM+Sans:wght@400;500;600;700&display=swap');
        *{box-sizing:border-box;margin:0;padding:0}
        ::-webkit-scrollbar{width:5px}::-webkit-scrollbar-thumb{background:#2563EB;border-radius:3px}
        .tc{transition:all .24s cubic-bezier(.34,1.56,.64,1);cursor:pointer}
        .tc:hover{transform:translateY(-5px) scale(1.025);box-shadow:0 14px 32px rgba(0,0,0,.12)!important}
        .bp{background:linear-gradient(135deg,#1d4ed8,#2563EB);color:#fff;border:none;padding:12px 28px;border-radius:11px;font-weight:700;font-size:15px;cursor:pointer;transition:all .2s;font-family:inherit}
        .bp:hover:not(:disabled){transform:translateY(-2px);box-shadow:0 10px 24px rgba(37,99,235,.4)}
        .bp:disabled{opacity:.5;cursor:not-allowed;transform:none!important}
        .bo{background:transparent;border:1.5px solid ${br};color:${tx};padding:10px 22px;border-radius:10px;font-weight:500;font-size:14px;cursor:pointer;transition:all .18s;font-family:inherit}
        .bo:hover{border-color:#2563EB;color:#2563EB;background:rgba(37,99,235,.05)}
        .gtext{background:linear-gradient(135deg,#1d4ed8,#7C3AED,#EC4899);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
        .faq-q{width:100%;text-align:left;padding:20px 0;background:none;border:none;cursor:pointer;font-family:inherit;display:flex;justify-content:space-between;align-items:center;gap:12px}
        .dl-btn{background:linear-gradient(135deg,#059669,#10B981);color:#fff;border:none;padding:9px 20px;border-radius:10px;font-weight:700;font-size:13px;cursor:pointer;font-family:inherit;flex-shrink:0;transition:all .18s}
        .dl-btn:hover{transform:translateY(-1px);box-shadow:0 8px 20px rgba(16,185,129,.4)}
        .pline{height:5px;border-radius:3px;background:linear-gradient(90deg,#1d4ed8,#7C3AED,#10B981);background-size:200%;animation:slide 1.4s linear infinite}
        @keyframes slide{0%{background-position:100%}100%{background-position:-100%}}
        .modal-bg{position:fixed;inset:0;background:rgba(0,0,0,.7);backdrop-filter:blur(12px);z-index:999;display:flex;align-items:center;justify-content:center;padding:16px}
        .modal-box{animation:pop .3s cubic-bezier(.34,1.56,.64,1)}
        @keyframes pop{from{transform:scale(.85) translateY(20px);opacity:0}to{transform:scale(1) translateY(0);opacity:1}}
        .toast{animation:tin .35s cubic-bezier(.34,1.56,.64,1) forwards}
        @keyframes tin{from{transform:translateX(130%);opacity:0}to{transform:translateX(0);opacity:1}}
        .drop-zone{transition:all .2s cubic-bezier(.34,1.56,.64,1)}
        .drop-zone:hover,.drop-zone.dragging{transform:scale(1.01);border-color:#2563EB!important;background:rgba(37,99,235,.06)!important;box-shadow:0 0 0 5px rgba(37,99,235,.12)}
        .chip{animation:chip 2.8s ease-in-out infinite;font-size:13px;font-weight:600;padding:9px 15px;border-radius:12px;white-space:nowrap}
        .chip:nth-child(2){animation-delay:.7s}
        .chip:nth-child(3){animation-delay:1.4s}
        @keyframes chip{0%,100%{transform:translateY(0)}50%{transform:translateY(-10px)}}
        .fa{animation:fa .25s ease forwards}
        @keyframes fa{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:translateY(0)}}
        .inp{background:${d?"#0D1117":"#F6F8FA"};border:1.5px solid ${br};color:${tx};padding:9px 12px;border-radius:9px;font-size:14px;font-family:inherit;width:100%;outline:none;transition:border .18s}
        .inp:focus{border-color:#2563EB}
        input[type=range]{accent-color:#2563EB;width:100%}
        .pc{transition:all .28s cubic-bezier(.34,1.56,.64,1)}
        .pc:hover{transform:translateY(-7px)}
        .nl{cursor:pointer;font-weight:500;font-size:15px;transition:color .15s}
        .nl:hover{color:#2563EB}
      `}</style>

      {/* TOAST */}
      {toast && (
        <div className="toast" style={{ position:"fixed",top:20,right:20,zIndex:9999,
          background: toast.type==="err" ? "linear-gradient(135deg,#DC2626,#EF4444)" : "linear-gradient(135deg,#059669,#10B981)",
          color:"#fff",padding:"13px 20px",borderRadius:13,boxShadow:"0 20px 40px rgba(0,0,0,.25)",
          fontWeight:600,fontSize:14,maxWidth:340,lineHeight:1.4,pointerEvents:"none" }}>
          {toast.msg}
        </div>
      )}

      {/* ── MODAL ── */}
      {activeTool && (
        <div className="modal-bg" onClick={() => setActiveTool(null)}>
          <div className="modal-box" onClick={e => e.stopPropagation()}
            style={{ background:cd,borderRadius:24,padding:"34px",maxWidth:520,width:"100%",
              maxHeight:"92vh",overflowY:"auto",boxShadow:"0 50px 100px rgba(0,0,0,.4)",
              border:`1px solid ${br}`,position:"relative" }}>

            {/* Header */}
            <div style={{ display:"flex",alignItems:"center",gap:14,marginBottom:22 }}>
              <div style={{ width:52,height:52,borderRadius:15,background:activeTool.color+"1a",
                display:"flex",alignItems:"center",justifyContent:"center",fontSize:24,flexShrink:0 }}>
                {activeTool.icon}
              </div>
              <div style={{ flex:1,minWidth:0 }}>
                <h2 style={{ fontWeight:800,fontSize:19,letterSpacing:"-.03em",marginBottom:3 }}>{activeTool.name}</h2>
                <p style={{ color:ts,fontSize:13 }}>{activeTool.desc}</p>
              </div>
              <button onClick={() => setActiveTool(null)}
                style={{ background:"none",border:"none",cursor:"pointer",color:ts,fontSize:22,lineHeight:1,padding:4,flexShrink:0 }}>✕</button>
            </div>

            {/* Server-only notice */}
            {!activeTool.works && (
              <div style={{ background:"rgba(245,158,11,.1)",border:"1px solid rgba(245,158,11,.35)",
                borderRadius:10,padding:"12px 14px",color:"#B45309",fontSize:13,fontWeight:500,marginBottom:18 }}>
                ⚠️ <b>{activeTool.name}</b> requires server-side processing. In this browser demo only the 10 client-side tools work (JPG↔PDF, Merge, Split, Compress, Rotate, Watermark, Page Numbers, Unlock, Repair).
              </div>
            )}

            {/* Library loading status */}
            {activeTool.works && !(activeTool.id === "pdf-to-jpg" ? libOk.pdfjs : libOk.pdfLib) && (
              <div style={{ background:"rgba(37,99,235,.08)",border:"1px solid rgba(37,99,235,.2)",
                borderRadius:10,padding:"11px 14px",color:ac,fontSize:13,fontWeight:500,marginBottom:16 }}>
                ⏳ Loading PDF library… <span style={{ opacity:.65 }}>This only happens once per session.</span>
              </div>
            )}

            {/* Drop zone */}
            {activeTool.works && (
              <div className={`drop-zone${dropModal?" dragging":""}`}
                style={{ border:`2px dashed ${dropModal?ac:br}`,borderRadius:16,padding:"28px 16px",
                  textAlign:"center",cursor:"pointer",marginBottom:18,background:d?"rgba(255,255,255,.02)":"#F8FAFC" }}
                onDragOver={e => { e.preventDefault(); setDropModal(true); }}
                onDragLeave={() => setDropModal(false)}
                onDrop={e => { e.preventDefault(); setDropModal(false); addFiles(e.dataTransfer.files); }}
                onClick={() => modalInputRef.current?.click()}>
                <input ref={modalInputRef} type="file" multiple={activeTool.multi}
                  accept={activeTool.accept} style={{ display:"none" }}
                  onChange={e => addFiles(e.target.files)} />
                <div style={{ fontSize:38,marginBottom:9 }}>{dropModal ? "⬇️" : "📂"}</div>
                <p style={{ fontWeight:700,fontSize:15,marginBottom:4 }}>
                  {dropModal ? "Release to upload!" : `Drop ${activeTool.multi ? "files" : "file"} or click to browse`}
                </p>
                <p style={{ color:ts,fontSize:12,marginBottom:16 }}>
                  Accepted: {activeTool.accept.toUpperCase().replace(/\./g,"")}
                </p>
                <button className="bp" style={{ background:`linear-gradient(135deg,${activeTool.color}cc,${activeTool.color})`,padding:"10px 24px",fontSize:14 }}
                  onClick={e => { e.stopPropagation(); modalInputRef.current?.click(); }}>
                  Choose File{activeTool.multi ? "s" : ""}
                </button>
              </div>
            )}

            {/* File list */}
            {files.length > 0 && (
              <div style={{ marginBottom:16 }}>
                <div style={{ display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:8 }}>
                  <span style={{ fontWeight:600,fontSize:12,color:ts }}>
                    {files.length} file{files.length>1?"s":""} ready
                  </span>
                  <button onClick={() => setFiles([])} style={{ background:"none",border:"none",cursor:"pointer",color:ts,fontSize:12 }}>Clear all</button>
                </div>
                {files.map((f, i) => (
                  <div key={i} style={{ display:"flex",alignItems:"center",justifyContent:"space-between",
                    background:d?"#0D1117":"#F1F5F9",borderRadius:9,padding:"8px 12px",marginBottom:5,
                    border:`1px solid ${br}` }}>
                    <div style={{ display:"flex",alignItems:"center",gap:8,minWidth:0 }}>
                      <span style={{ fontSize:16 }}>{f.type.startsWith("image") ? "🖼" : "📄"}</span>
                      <span style={{ fontSize:13,fontWeight:500,overflow:"hidden",textOverflow:"ellipsis",whiteSpace:"nowrap" }}>{f.name}</span>
                    </div>
                    <span style={{ fontSize:12,color:ts,flexShrink:0,marginLeft:8 }}>{fmt(f.size)}</span>
                  </div>
                ))}
              </div>
            )}

            {/* Tool options */}
            {activeTool.id === "rotate" && files.length > 0 && (
              <div style={{ marginBottom:16 }}>
                <p style={{ fontWeight:600,fontSize:13,marginBottom:9 }}>Rotation angle</p>
                <div style={{ display:"flex",gap:8,flexWrap:"wrap" }}>
                  {[90,180,270].map(a => (
                    <button key={a} onClick={() => setOpts(p => ({...p, angle:a}))}
                      style={{ padding:"8px 16px",borderRadius:9,border:`1.5px solid ${opts.angle===a?activeTool.color:br}`,
                        background:opts.angle===a?activeTool.color+"18":"transparent",
                        color:opts.angle===a?activeTool.color:ts,fontWeight:600,fontSize:13,cursor:"pointer",fontFamily:"inherit" }}>
                      {a}° {a===90?"↻":a===180?"↕":"↺"}
                    </button>
                  ))}
                </div>
              </div>
            )}

            {activeTool.id === "watermark" && files.length > 0 && (
              <div style={{ marginBottom:16 }}>
                <p style={{ fontWeight:600,fontSize:13,marginBottom:7 }}>Watermark text</p>
                <input className="inp" value={opts.wmText}
                  onChange={e => setOpts(p => ({...p, wmText:e.target.value}))}
                  placeholder="CONFIDENTIAL" style={{ marginBottom:10 }} />
                <p style={{ fontWeight:600,fontSize:13,marginBottom:6 }}>Opacity — {Math.round(opts.wmOpacity*100)}%</p>
                <input type="range" min=".05" max=".9" step=".05" value={opts.wmOpacity}
                  onChange={e => setOpts(p => ({...p, wmOpacity:+e.target.value}))} />
              </div>
            )}

            {activeTool.id === "split" && files.length > 0 && (
              <div style={{ marginBottom:16 }}>
                <p style={{ fontWeight:600,fontSize:13,marginBottom:7 }}>Page range (leave blank for all pages)</p>
                <input className="inp" value={opts.splitRange}
                  onChange={e => setOpts(p => ({...p, splitRange:e.target.value}))}
                  placeholder="e.g. 1,3,5-8  — blank = every page" />
              </div>
            )}

            {/* Progress */}
            {busy && (
              <div style={{ marginBottom:16 }}>
                <div style={{ display:"flex",justifyContent:"space-between",fontSize:13,fontWeight:600,marginBottom:6 }}>
                  <span style={{ color:ts }}>Processing…</span>
                  <span style={{ color:activeTool.color }}>{prog}%</span>
                </div>
                <div style={{ background:d?"#0D1117":"#E2E8F0",borderRadius:4,overflow:"hidden",height:5 }}>
                  <div className="pline" style={{ width:`${prog}%`,transition:"width .25s" }} />
                </div>
              </div>
            )}

            {/* Error */}
            {errMsg && (
              <div className="fa" style={{ background:"rgba(220,38,38,.08)",border:"1px solid rgba(220,38,38,.28)",
                borderRadius:10,padding:"11px 14px",color:"#EF4444",fontSize:13,fontWeight:500,marginBottom:14 }}>
                ❌ {errMsg}
              </div>
            )}

            {/* Results */}
            {results.length > 0 && (
              <div className="fa" style={{ marginBottom:16 }}>
                <div style={{ display:"flex",alignItems:"center",justifyContent:"space-between",marginBottom:10 }}>
                  <span style={{ fontWeight:700,fontSize:14,color:"#10B981" }}>
                    ✅ {results.length} file{results.length>1?"s":""} ready!
                  </span>
                  {results.length > 1 && (
                    <button className="dl-btn" onClick={() => results.forEach((r,i) => setTimeout(() => downloadResult(r), i*250))}>
                      Download All
                    </button>
                  )}
                </div>
                {results.map((r, i) => (
                  <div key={i} style={{ background:d?"#0D1117":"#F0FDF4",border:"1px solid #10B98128",
                    borderRadius:11,padding:"10px 14px",marginBottom:8 }}>
                    <div style={{ display:"flex",alignItems:"center",justifyContent:"space-between",gap:10 }}>
                      <div style={{ minWidth:0 }}>
                        <p style={{ fontWeight:600,fontSize:13,marginBottom:r.note?4:0 }}>
                          {r.isImage?"🖼":"📄"} {r.name}
                        </p>
                        {r.note && <p style={{ color:ts,fontSize:11 }}>{r.note}</p>}
                      </div>
                      <button className="dl-btn" onClick={() => downloadResult(r)}>⬇ Save</button>
                    </div>
                    {r.isImage && r.dataUrl && (
                      <img src={r.dataUrl} alt={r.name}
                        style={{ marginTop:8,maxWidth:"100%",maxHeight:150,borderRadius:7,display:"block",objectFit:"contain" }} />
                    )}
                  </div>
                ))}
              </div>
            )}

            {/* Process button */}
            {activeTool.works && (
              <button className="bp" disabled={!canProcess}
                style={{ width:"100%",padding:"14px",fontSize:15,borderRadius:13,
                  background: !canProcess ? (d?"#2D3748":"#CBD5E1") : `linear-gradient(135deg,${activeTool.color}cc,${activeTool.color})` }}
                onClick={runTool}>
                {busy ? `Processing… ${prog}%`
                  : !(activeTool.id==="pdf-to-jpg"?libOk.pdfjs:libOk.pdfLib) ? "⏳ Loading library…"
                  : !files.length ? "Upload a file first"
                  : `🚀 Run ${activeTool.name}`}
              </button>
            )}
            <p style={{ color:ts,fontSize:11,textAlign:"center",marginTop:10 }}>
              🔒 Files stay in your browser · Never uploaded to any server
            </p>
          </div>
        </div>
      )}

      {/* ── NAV ── */}
      <nav style={{ position:"fixed",top:0,left:0,right:0,zIndex:100,
        background:d?"rgba(13,17,23,.95)":"rgba(255,255,255,.95)",
        backdropFilter:"blur(20px)",borderBottom:`1px solid ${br}`,height:62,
        display:"flex",alignItems:"center",padding:"0 20px" }}>
        <div style={{ maxWidth:1280,margin:"0 auto",width:"100%",display:"flex",alignItems:"center",justifyContent:"space-between" }}>
          <div style={{ display:"flex",alignItems:"center",gap:9 }}>
            <div style={{ width:34,height:34,borderRadius:9,background:"linear-gradient(135deg,#1d4ed8,#7C3AED)",
              display:"flex",alignItems:"center",justifyContent:"center",color:"#fff",fontWeight:800,fontSize:16 }}>P</div>
            <span style={{ fontWeight:800,fontSize:18,letterSpacing:"-.04em" }}>PDF<span style={{ color:ac }}>Craft</span></span>
          </div>
          <div style={{ display:"flex",gap:28 }}>
            {["Tools","Pricing","Blog","API","Docs"].map(l => (
              <span key={l} className="nl" style={{ color:ts }}>{l}</span>
            ))}
          </div>
          <div style={{ display:"flex",gap:8,alignItems:"center" }}>
            {/* Lib status pill */}
            <div style={{ display:"flex",alignItems:"center",gap:5,background:d?"#1a2332":"#EFF6FF",
              border:`1px solid ${br}`,borderRadius:100,padding:"4px 11px",fontSize:11,fontWeight:600 }}>
              <span style={{ color:libOk.pdfLib?"#10B981":"#F59E0B" }}>●</span>
              <span style={{ color:ts }}>pdf-lib</span>
              <span style={{ color:libOk.pdfjs?"#10B981":"#F59E0B" }}>●</span>
              <span style={{ color:ts }}>pdf.js</span>
            </div>
            <button onClick={() => setDark(!d)}
              style={{ width:36,height:36,borderRadius:9,background:d?"#21262D":"#F1F5F9",
                border:`1px solid ${br}`,cursor:"pointer",fontSize:16,display:"flex",alignItems:"center",justifyContent:"center" }}>
              {d?"☀️":"🌙"}
            </button>
            <button className="bp" style={{ padding:"8px 20px",fontSize:13 }} onClick={() => showToast("🎉 Opening sign up…")}>
              Get Started
            </button>
          </div>
        </div>
      </nav>

      {/* ── HERO ── */}
      <section style={{ paddingTop:120,paddingBottom:64,position:"relative",overflow:"hidden",
        background:d?"#0D1117":"#F6F8FA" }}>
        <div style={{ position:"absolute",inset:0,
          background:"radial-gradient(ellipse at 18% 55%,rgba(37,99,235,.10) 0%,transparent 58%),radial-gradient(ellipse at 82% 20%,rgba(124,58,237,.08) 0%,transparent 50%)" }} />

        {/* Floating chips */}
        <div style={{ position:"absolute",top:100,left:"4%",display:"flex",flexDirection:"column",gap:8 }}>
          {[
            { c:"#3B82F6", t:"📄 report.pdf → Compressed 71%" },
            { c:"#10B981", t:"✅ 4 PDFs merged → merged.pdf" },
          ].map((ch,i) => (
            <div key={i} className="chip" style={{ background:d?"#161B22":"#fff",
              border:`1px solid ${br}`,boxShadow:"0 12px 30px rgba(0,0,0,.1)",color:ch.c }}>
              {ch.t}
            </div>
          ))}
        </div>
        <div style={{ position:"absolute",top:130,right:"4%" }}>
          <div className="chip" style={{ background:d?"#161B22":"#fff",
            border:`1px solid ${br}`,boxShadow:"0 12px 30px rgba(0,0,0,.1)",color:"#7C3AED",animationDelay:"1.4s" }}>
            🖼 photo.jpg → PDF in 0.3s
          </div>
        </div>

        <div style={{ maxWidth:820,margin:"0 auto",textAlign:"center",padding:"0 20px",position:"relative" }}>
          <div style={{ display:"inline-flex",alignItems:"center",gap:7,
            background:"rgba(37,99,235,.10)",border:"1px solid rgba(37,99,235,.25)",
            borderRadius:100,padding:"5px 16px",fontSize:13,fontWeight:600,color:ac,marginBottom:24 }}>
            <span style={{ width:6,height:6,borderRadius:"50%",background:ac,display:"inline-block" }}/>
            10 Tools Run 100% in Your Browser · Zero Upload
          </div>

          <h1 style={{ fontSize:"clamp(34px,6vw,68px)",fontWeight:800,lineHeight:1.08,letterSpacing:"-.04em",marginBottom:18 }}>
            Every tool you need to<br/>
            <span className="gtext">work with PDFs</span><br/>
            in one place
          </h1>
          <p style={{ fontSize:"clamp(15px,2vw,19px)",color:ts,maxWidth:520,margin:"0 auto 32px",lineHeight:1.65 }}>
            Merge, split, compress, rotate, convert JPG↔PDF and more — processed locally in your browser. Fast, private, no server.
          </p>

          {/* Hero drop zone */}
          <div className={`drop-zone${dropHero?" dragging":""}`}
            onDragOver={e => { e.preventDefault(); setDropHero(true); }}
            onDragLeave={() => setDropHero(false)}
            onDrop={e => {
              e.preventDefault(); setDropHero(false);
              const arr = Array.from(e.dataTransfer.files);
              if (arr.length) showToast(`📂 ${arr.length} file(s) ready! Pick a tool below.`);
            }}
            onClick={() => heroInputRef.current?.click()}
            style={{ border:`2px dashed ${dropHero?ac:br}`,borderRadius:20,padding:"44px 28px",
              background:d?"rgba(255,255,255,.02)":"#FFFFFF",cursor:"pointer",marginBottom:22,
              boxShadow:"0 4px 20px rgba(0,0,0,.06)" }}>
            <input ref={heroInputRef} type="file" multiple accept=".pdf,image/*"
              style={{ display:"none" }}
              onChange={e => { if(e.target.files?.length) showToast(`📂 ${e.target.files.length} file(s) ready! Pick a tool below.`); }} />
            <div style={{ fontSize:48,marginBottom:10 }}>{dropHero?"⬇️":"📁"}</div>
            <p style={{ fontWeight:700,fontSize:17,marginBottom:5 }}>
              {dropHero ? "Release to upload!" : "Drag & Drop PDF or Image files here"}
            </p>
            <p style={{ color:ts,fontSize:13,marginBottom:20 }}>PDF, JPG, PNG, DOCX, XLS, PPT — or click to browse</p>
            <button className="bp" onClick={e => { e.stopPropagation(); heroInputRef.current?.click(); }}>
              Choose Files
            </button>
            <p style={{ color:ts,fontSize:11,marginTop:12 }}>🔒 All processing runs locally — files never leave your device</p>
          </div>

          <div style={{ display:"flex",justifyContent:"center",gap:22,flexWrap:"wrap",color:ts,fontSize:13 }}>
            {["⚡ Instant","🔒 100% Private","🌍 GDPR-Safe","📱 Any Device"].map(f => (
              <span key={f}>{f}</span>
            ))}
          </div>
        </div>
      </section>

      {/* Library loading banner */}
      {!libAllReady && (
        <div style={{ background:"linear-gradient(135deg,#1d4ed8,#7C3AED)",padding:"12px 24px",textAlign:"center",color:"#fff",fontSize:14,fontWeight:500 }}>
          ⏳ Loading PDF libraries in background ({libOk.pdfLib?"✅":"⏳"} pdf-lib · {libOk.pdfjs?"✅":"⏳"} pdf.js) — tools will enable automatically when ready.
        </div>
      )}
      {libAllReady && (
        <div style={{ background:"linear-gradient(135deg,#059669,#10B981)",padding:"10px 24px",textAlign:"center",color:"#fff",fontSize:13,fontWeight:600 }}>
          ✅ All PDF libraries loaded — every client-side tool is ready to use!
        </div>
      )}

      {/* STATS */}
      <div style={{ background:"linear-gradient(135deg,#1e3a8a,#2563EB,#7c3aed)",padding:"24px 20px" }}>
        <div style={{ maxWidth:1000,margin:"0 auto",display:"flex",justifyContent:"space-around",flexWrap:"wrap",gap:14 }}>
          {[["50M+","Files Processed"],["4.9★","User Rating"],["10","Browser Tools"],["0 KB","Uploaded to Server"]].map(([v,l]) => (
            <div key={l} style={{ textAlign:"center",color:"#fff" }}>
              <div style={{ fontSize:28,fontWeight:800,letterSpacing:"-.03em" }}>{v}</div>
              <div style={{ fontSize:12,opacity:.72,marginTop:2 }}>{l}</div>
            </div>
          ))}
        </div>
      </div>

      {/* ── TOOLS GRID ── */}
      <section style={{ padding:"72px 20px",background:bg }}>
        <div style={{ maxWidth:1280,margin:"0 auto" }}>
          <div style={{ textAlign:"center",marginBottom:40 }}>
            <h2 style={{ fontSize:"clamp(24px,4vw,44px)",fontWeight:800,letterSpacing:"-.03em",marginBottom:10 }}>
              All PDF Tools — Click to Start
            </h2>
            <p style={{ color:ts,fontSize:15,maxWidth:480,margin:"0 auto" }}>
              Green border = runs in your browser. Orange = demo only (needs server).
            </p>
          </div>

          <div style={{ display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(195px,1fr))",gap:13 }}>
            {TOOLS.map(t => (
              <div key={t.id} className="tc" onClick={() => openModal(t)}
                onMouseEnter={() => setHov(t.id)} onMouseLeave={() => setHov(null)}
                style={{ background:cd,
                  border:`2px solid ${hov===t.id ? t.color : t.works ? "#10B98130" : "#F59E0B22"}`,
                  borderRadius:16,padding:"20px 16px",position:"relative",
                  boxShadow:hov===t.id?`0 10px 26px ${t.color}1a`:"0 2px 6px rgba(0,0,0,.04)",
                  opacity: t.works ? 1 : 0.72 }}>
                <div style={{ position:"absolute",top:11,right:11,
                  background: t.works ? "#DCFCE7" : "#FEF3C7",
                  color: t.works ? "#15803D" : "#B45309",
                  fontSize:9,fontWeight:800,padding:"2px 7px",borderRadius:100,textTransform:"uppercase" }}>
                  {t.works ? "✓ Live" : "Server"}
                </div>
                <div style={{ width:48,height:48,borderRadius:13,background:t.color+"18",
                  display:"flex",alignItems:"center",justifyContent:"center",
                  fontSize:20,marginBottom:12,color:t.color,fontWeight:800 }}>{t.icon}</div>
                <h3 style={{ fontWeight:700,fontSize:14,marginBottom:4,letterSpacing:"-.01em" }}>{t.name}</h3>
                <p style={{ color:ts,fontSize:12,lineHeight:1.5 }}>{t.desc}</p>
                <div style={{ marginTop:12,color:t.color,fontSize:12,fontWeight:700 }}>
                  {t.works ? "Use Tool →" : "View Demo →"}
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* HOW IT WORKS */}
      <section style={{ padding:"72px 20px",background:sf }}>
        <div style={{ maxWidth:860,margin:"0 auto",textAlign:"center" }}>
          <h2 style={{ fontSize:"clamp(22px,4vw,40px)",fontWeight:800,letterSpacing:"-.03em",marginBottom:10 }}>
            3 Steps — Done
          </h2>
          <p style={{ color:ts,fontSize:15,marginBottom:48 }}>No account, no install, no upload to any server</p>
          <div style={{ display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(220px,1fr))",gap:14 }}>
            {[
              { n:"01",icon:"📂",t:"Upload Your File",d:"Drop a PDF or image onto the tool. Files stay 100% in your browser — never sent to any server." },
              { n:"02",icon:"⚙️",t:"Pick a Tool",d:"Click any green-bordered tool. Adjust options like rotation angle, watermark text, or page range." },
              { n:"03",icon:"⬇️",t:"Download Instantly",d:"Click Save to download the processed file directly to your device. Done in seconds." },
            ].map(s => (
              <div key={s.n} style={{ background:cd,borderRadius:18,padding:"32px 22px",
                border:`1px solid ${br}`,position:"relative",overflow:"hidden" }}>
                <div style={{ position:"absolute",top:-8,right:-4,fontSize:72,fontWeight:900,
                  color:ts,opacity:.04,letterSpacing:-3,lineHeight:1 }}>{s.n}</div>
                <div style={{ width:58,height:58,borderRadius:16,
                  background:"linear-gradient(135deg,#1d4ed8,#2563EB)",
                  display:"flex",alignItems:"center",justifyContent:"center",
                  fontSize:28,margin:"0 auto 16px" }}>{s.icon}</div>
                <div style={{ display:"inline-block",background:"rgba(37,99,235,.1)",color:ac,
                  borderRadius:100,padding:"2px 11px",fontSize:10,fontWeight:700,marginBottom:10 }}>STEP {s.n}</div>
                <h3 style={{ fontWeight:700,fontSize:16,marginBottom:7 }}>{s.t}</h3>
                <p style={{ color:ts,fontSize:13,lineHeight:1.65 }}>{s.d}</p>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* PRICING */}
      <section style={{ padding:"72px 20px",background:bg }}>
        <div style={{ maxWidth:1000,margin:"0 auto" }}>
          <div style={{ textAlign:"center",marginBottom:40 }}>
            <h2 style={{ fontSize:"clamp(22px,4vw,40px)",fontWeight:800,letterSpacing:"-.03em",marginBottom:10 }}>
              Simple Pricing
            </h2>
            <div style={{ display:"inline-flex",background:cd,borderRadius:11,padding:4,border:`1px solid ${br}`,marginTop:16 }}>
              {["monthly","yearly"].map(c => (
                <button key={c} onClick={() => setBilling(c)}
                  style={{ padding:"7px 20px",borderRadius:8,border:"none",fontFamily:"inherit",fontWeight:700,fontSize:13,cursor:"pointer",
                    background:billing===c?"linear-gradient(135deg,#1d4ed8,#2563EB)":"transparent",
                    color:billing===c?"#fff":ts,position:"relative",transition:"all .18s" }}>
                  {c==="monthly"?"Monthly":"Yearly"}
                  {c==="yearly" && <span style={{ position:"absolute",top:-8,right:-8,background:"#10B981",color:"#fff",fontSize:8,fontWeight:800,borderRadius:100,padding:"2px 6px" }}>-25%</span>}
                </button>
              ))}
            </div>
          </div>
          <div style={{ display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(260px,1fr))",gap:16,alignItems:"center" }}>
            {[
              { name:"Free",     p:0,  pop:false, color:"#6B7280", feats:["5 files per task","2 tasks / day","Max 100 MB","Basic tools","Watermark on output"],             cta:"Get Started" },
              { name:"Pro",      p:9,  pop:true,  color:"#2563EB", feats:["Unlimited files","All 20+ tools","Max 2 GB","Priority processing","No watermarks","10 GB cloud","API access"], cta:"Start Free Trial" },
              { name:"Business", p:29, pop:false, color:"#7C3AED", feats:["Everything in Pro","Team workspace","Max 5 GB","50 GB cloud","Custom branding","Priority support","Batch API"],  cta:"Contact Sales" },
            ].map(pl => (
              <div key={pl.name} className="pc"
                style={{ background:pl.pop?"linear-gradient(160deg,#1e3a8a,#2563EB,#3b82f6)":cd,
                  borderRadius:22,padding:pl.pop?"42px 28px":"34px 26px",
                  border:pl.pop?"none":`1.5px solid ${br}`,
                  boxShadow:pl.pop?"0 28px 56px rgba(37,99,235,.35)":"0 4px 14px rgba(0,0,0,.05)",
                  position:"relative",overflow:"hidden" }}>
                {pl.pop && <div style={{ position:"absolute",top:16,right:16,
                  background:"rgba(255,255,255,.2)",color:"#fff",borderRadius:100,padding:"3px 12px",fontSize:11,fontWeight:700 }}>
                  ⭐ Popular</div>}
                <h3 style={{ fontWeight:800,fontSize:18,color:pl.pop?"#fff":tx,marginBottom:6 }}>{pl.name}</h3>
                <div style={{ display:"flex",alignItems:"baseline",gap:4,marginBottom:22 }}>
                  <span style={{ fontSize:44,fontWeight:900,letterSpacing:"-.04em",color:pl.pop?"#fff":tx }}>
                    ${billing==="yearly"?Math.floor(pl.p*.75):pl.p}
                  </span>
                  <span style={{ fontSize:13,color:pl.pop?"rgba(255,255,255,.6)":ts }}>
                    {pl.p===0?"/forever":billing==="yearly"?"/mo billed yr":"/month"}
                  </span>
                </div>
                <div style={{ marginBottom:26 }}>
                  {pl.feats.map(f => (
                    <div key={f} style={{ display:"flex",alignItems:"flex-start",gap:9,marginBottom:8 }}>
                      <span style={{ color:"#10B981",fontWeight:700,fontSize:14 }}>✓</span>
                      <span style={{ fontSize:13,color:pl.pop?"rgba(255,255,255,.82)":ts }}>{f}</span>
                    </div>
                  ))}
                </div>
                <button onClick={() => showToast(`Starting ${pl.name} plan…`)}
                  style={{ width:"100%",padding:"13px",borderRadius:11,fontWeight:700,fontSize:14,cursor:"pointer",fontFamily:"inherit",
                    background:pl.pop?"rgba(255,255,255,.15)":"linear-gradient(135deg,#1d4ed8,#2563EB)",
                    border:pl.pop?"1.5px solid rgba(255,255,255,.25)":"none",color:"#fff" }}>
                  {pl.cta}
                </button>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* FAQ */}
      <section style={{ padding:"72px 20px",background:sf }}>
        <div style={{ maxWidth:720,margin:"0 auto" }}>
          <h2 style={{ fontSize:"clamp(22px,4vw,38px)",fontWeight:800,letterSpacing:"-.03em",textAlign:"center",marginBottom:40 }}>FAQ</h2>
          {FAQS.map((f, i) => (
            <div key={i} style={{ borderBottom:`1px solid ${br}` }}>
              <button className="faq-q" onClick={() => setOpenFaq(openFaq===i?null:i)}>
                <span style={{ fontWeight:600,fontSize:15,color:tx,textAlign:"left" }}>{f.q}</span>
                <span style={{ color:ac,fontSize:20,transition:"transform .18s",transform:openFaq===i?"rotate(45deg)":"none",flexShrink:0 }}>+</span>
              </button>
              {openFaq===i && (
                <div className="fa" style={{ paddingBottom:18,color:ts,fontSize:14,lineHeight:1.75 }}>{f.a}</div>
              )}
            </div>
          ))}
        </div>
      </section>

      {/* CTA */}
      <section style={{ padding:"64px 20px",background:bg }}>
        <div style={{ maxWidth:820,margin:"0 auto" }}>
          <div style={{ background:"linear-gradient(135deg,#1e3a8a,#1d4ed8,#2563eb,#7c3aed)",
            borderRadius:24,padding:"56px 40px",textAlign:"center",overflow:"hidden",position:"relative",
            boxShadow:"0 36px 72px rgba(37,99,235,.35)" }}>
            <div style={{ position:"absolute",inset:0,background:"radial-gradient(ellipse at 30% 50%,rgba(255,255,255,.07) 0%,transparent 60%)" }}/>
            <div style={{ position:"relative" }}>
              <h2 style={{ fontSize:"clamp(22px,4vw,44px)",fontWeight:800,color:"#fff",letterSpacing:"-.03em",marginBottom:12 }}>
                Ready to streamline your PDF workflow?
              </h2>
              <p style={{ color:"rgba(255,255,255,.7)",fontSize:16,marginBottom:30 }}>
                Join 2M+ professionals. Start free, upgrade when you need more.
              </p>
              <div style={{ display:"flex",gap:12,justifyContent:"center",flexWrap:"wrap" }}>
                <button className="bp" onClick={() => showToast("🎉 Creating your free account!")}
                  style={{ background:"#fff",color:"#1d4ed8",padding:"14px 34px",fontSize:15 }}>
                  Start for Free
                </button>
                <button onClick={() => showToast("Opening demo…")}
                  style={{ background:"rgba(255,255,255,.12)",border:"1.5px solid rgba(255,255,255,.25)",
                    color:"#fff",padding:"14px 34px",borderRadius:11,fontSize:15,fontWeight:600,cursor:"pointer",fontFamily:"inherit" }}>
                  Watch Demo →
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* FOOTER */}
      <footer style={{ background:d?"#010409":"#0D1117",color:"rgba(255,255,255,.55)",padding:"52px 20px 24px" }}>
        <div style={{ maxWidth:1200,margin:"0 auto" }}>
          <div style={{ display:"grid",gridTemplateColumns:"2fr repeat(4,1fr)",gap:32,marginBottom:36,flexWrap:"wrap" }}>
            <div>
              <div style={{ display:"flex",alignItems:"center",gap:8,marginBottom:12 }}>
                <div style={{ width:32,height:32,borderRadius:8,background:"linear-gradient(135deg,#1d4ed8,#7C3AED)",
                  display:"flex",alignItems:"center",justifyContent:"center",color:"#fff",fontWeight:800,fontSize:15 }}>P</div>
                <span style={{ fontWeight:800,fontSize:17,color:"#fff",letterSpacing:"-.04em" }}>
                  PDF<span style={{ color:"#3B82F6" }}>Craft</span>
                </span>
              </div>
              <p style={{ fontSize:13,lineHeight:1.65,maxWidth:200,marginBottom:16 }}>
                Complete PDF toolkit. Fast, private, runs in your browser.
              </p>
              <div style={{ display:"flex",gap:8 }}>
                {["𝕏","in","f","▶"].map(s=>(
                  <div key={s} style={{ width:32,height:32,borderRadius:7,background:"rgba(255,255,255,.07)",
                    display:"flex",alignItems:"center",justifyContent:"center",cursor:"pointer",
                    fontSize:12,fontWeight:700,color:"#fff" }}>{s}</div>
                ))}
              </div>
            </div>
            {[
              { t:"Tools",   ls:["Merge PDF","Split PDF","Compress PDF","PDF to JPG","JPG to PDF"] },
              { t:"Convert", ls:["Rotate PDF","Watermark","Page Numbers","Unlock PDF","Repair PDF"] },
              { t:"Company", ls:["About","Blog","Careers","API Docs","Contact"] },
              { t:"Legal",   ls:["Privacy","Terms","Cookies","GDPR","Security"] },
            ].map(col => (
              <div key={col.t}>
                <h4 style={{ color:"#fff",fontWeight:700,fontSize:12,marginBottom:12,letterSpacing:"-.01em",textTransform:"uppercase",opacity:.5 }}>{col.t}</h4>
                {col.ls.map(l=>(
                  <div key={l} style={{ marginBottom:8,fontSize:13,cursor:"pointer",transition:"color .14s" }}
                    onMouseEnter={e=>e.target.style.color="#3B82F6"}
                    onMouseLeave={e=>e.target.style.color="rgba(255,255,255,.55)"}>{l}</div>
                ))}
              </div>
            ))}
          </div>
          <div style={{ borderTop:"1px solid rgba(255,255,255,.07)",paddingTop:22,
            display:"flex",justifyContent:"space-between",alignItems:"center",flexWrap:"wrap",gap:10,fontSize:12 }}>
            <span>© 2026 PDFCraft · All rights reserved · Processing happens in your browser, not our servers.</span>
            <div style={{ display:"flex",gap:16 }}>
              {["Privacy","Terms","Cookies"].map(l=><span key={l} style={{ cursor:"pointer" }}>{l}</span>)}
            </div>
          </div>
        </div>
      </footer>
    </div>
  );
}

// ─── FAQ data (used in the FAQ section above) ────────────────────────────────
const FAQS = [
  { q:"Are my files safe?",                a:"100%. All processing runs entirely in your browser using WebAssembly/JavaScript. Your files are never uploaded to any server, ever." },
  { q:"Which tools actually work in browser?", a:"10 tools run client-side: JPG→PDF, PDF→JPG, Merge, Split, Compress, Rotate, Watermark, Page Numbers, Unlock, and Repair. The rest (OCR, Word conversion, signing) need server processing." },
  { q:"Why does the first use take a moment?", a:"pdf-lib (~900 KB) and pdf.js (~1.2 MB) are loaded from CDN the first time. After that they're cached by your browser and load instantly." },
  { q:"Is there an API?",                  a:"Pro and Business users get a full RESTful API. SDKs for JS, Python, and PHP. See API docs for details." },
  { q:"What file size limits apply?",      a:"Free: 100 MB. Pro: 2 GB. Business: 5 GB. Client-side tools in the browser are limited by your available RAM." },
  { q:"What payment methods?",             a:"Visa, Mastercard, Amex, PayPal, Apple Pay, Google Pay — all secured by Stripe." },
];
