import React, { useState, useMemo, useRef, useEffect, useCallback } from "react";
import {
  Wallet,
  Brain,
  BookOpen,
  Target,
  NotebookPen,
  Radar,
  ScanLine,
  ChevronRight,
  Upload,
  Send,
  TrendingUp,
  TrendingDown,
  ShieldCheck,
  Sparkles,
  X,
  Loader2,
  Check,
  ArrowRight,
  LineChart as LineChartIcon,
  MessageCircle,
  Percent,
} from "lucide-react";

/* ------------------------------------------------------------------ */
/*  Arc Network — real wallet + USDC payment helpers                   */
/*  (raw window.ethereum / EIP-1193 — no external lib required)        */
/*                                                                      */
/*  Per Arc's docs (docs.arc.io/arc/references/contract-addresses):    */
/*  USDC is Arc's native gas token, but the native balance uses 18     */
/*  decimals of precision. Arc's own docs recommend integrations use   */
/*  the USDC ERC-20 interface instead (6 decimals, matching USDC       */
/*  everywhere else) for reading balances and sending transfers, to    */
/*  avoid mixing the two decimal systems. That's what this does.       */
/* ------------------------------------------------------------------ */
const ARC_TESTNET_CHAIN_ID_HEX = "0x4cef52"; // 5042002
const ARC_TESTNET_RPC_URL = "https://rpc.testnet.arc.network";
const ARC_TESTNET_EXPLORER_URL = "https://testnet.arcscan.app";

// USDC ERC-20 interface on Arc Testnet — an optional interface over the
// native USDC balance. Always 6 decimals. Verify against
// https://docs.arc.io/arc/references/contract-addresses before going live.
const USDC_ADDRESS = "0x3600000000000000000000000000000000000000";
const USDC_DECIMALS = 6;

// Standard ERC-20 function selectors (first 4 bytes of the keccak256 hash
// of the function signature) — well-known constants, same on every chain.
const SELECTOR_BALANCE_OF = "70a08231"; // balanceOf(address)
const SELECTOR_TRANSFER = "a9059cbb"; // transfer(address,uint256)

const ARC_TESTNET_ADD_CHAIN_PARAMS = {
  chainId: ARC_TESTNET_CHAIN_ID_HEX,
  chainName: "Arc Testnet",
  nativeCurrency: { name: "USDC", symbol: "USDC", decimals: 18 }, // native precision per Arc docs; the ERC-20 interface below uses 6
  rpcUrls: [ARC_TESTNET_RPC_URL],
  blockExplorerUrls: [ARC_TESTNET_EXPLORER_URL],
};

function formatUsdc(hexAmount) {
  const raw = BigInt(hexAmount);
  const divisor = 10n ** BigInt(USDC_DECIMALS);
  const whole = raw / divisor;
  const frac = (raw % divisor).toString().padStart(USDC_DECIMALS, "0").replace(/0+$/, "");
  return frac ? `${whole}.${frac}` : whole.toString();
}

function parseUsdc(amountStr) {
  const [whole, frac = ""] = String(amountStr).split(".");
  const fracPadded = (frac + "0".repeat(USDC_DECIMALS)).slice(0, USDC_DECIMALS);
  return BigInt(whole || "0") * 10n ** BigInt(USDC_DECIMALS) + BigInt(fracPadded || "0");
}

function encodeAddress(addr) {
  return addr.toLowerCase().replace("0x", "").padStart(64, "0");
}

function encodeUint256(value) {
  return value.toString(16).padStart(64, "0");
}

async function ensureArcTestnet() {
  try {
    await window.ethereum.request({
      method: "wallet_switchEthereumChain",
      params: [{ chainId: ARC_TESTNET_CHAIN_ID_HEX }],
    });
  } catch (err) {
    if (err && err.code === 4902) {
      await window.ethereum.request({
        method: "wallet_addEthereumChain",
        params: [ARC_TESTNET_ADD_CHAIN_PARAMS],
      });
    } else {
      throw err;
    }
  }
}

// Reads the USDC balance via the ERC-20 interface's balanceOf(address),
// not eth_getBalance, since the native balance uses a different (18)
// decimal precision than USDC's standard 6.
async function getUsdcBalance(address) {
  const data = "0x" + SELECTOR_BALANCE_OF + encodeAddress(address);
  const resultHex = await window.ethereum.request({
    method: "eth_call",
    params: [{ to: USDC_ADDRESS, data }, "latest"],
  });
  return formatUsdc(resultHex);
}

async function connectArcWallet() {
  if (!window.ethereum) {
    throw new Error("No wallet found. Install MetaMask, Rabby, or another browser wallet extension.");
  }
  const [address] = await window.ethereum.request({ method: "eth_requestAccounts" });
  const chainId = await window.ethereum.request({ method: "eth_chainId" });
  if (chainId !== ARC_TESTNET_CHAIN_ID_HEX) {
    await ensureArcTestnet();
  }
  const balance = await getUsdcBalance(address);
  return { address, balance };
}

async function refreshArcBalance(address) {
  return getUsdcBalance(address);
}

// Sends USDC via the ERC-20 transfer(address,uint256) call rather than a
// native value transfer, per Arc's own integration guidance.
async function payWithUsdc(fromAddress, merchantAddress, amountUsdc) {
  const amount = parseUsdc(amountUsdc);
  const data = "0x" + SELECTOR_TRANSFER + encodeAddress(merchantAddress) + encodeUint256(amount);

  const txHash = await window.ethereum.request({
    method: "eth_sendTransaction",
    params: [{ from: fromAddress, to: USDC_ADDRESS, data }],
  });

  // Arc reaches deterministic finality in under a second, but we still
  // poll briefly for the receipt rather than assuming success immediately.
  for (let i = 0; i < 20; i++) {
    const receipt = await window.ethereum.request({
      method: "eth_getTransactionReceipt",
      params: [txHash],
    });
    if (receipt) {
      return {
        txHash,
        status: receipt.status === "0x1" ? "confirmed" : "failed",
        explorerUrl: `${ARC_TESTNET_EXPLORER_URL}/tx/${txHash}`,
      };
    }
    await new Promise((r) => setTimeout(r, 750));
  }
  return { txHash, status: "pending", explorerUrl: `${ARC_TESTNET_EXPLORER_URL}/tx/${txHash}` };
}

function shortenAddress(addr) {
  return addr ? `${addr.slice(0, 6)}…${addr.slice(-4)}` : "";
}

// Placeholder merchant address — replace with your real Arc treasury/receiving
// wallet before going live.
const MERCHANT_ADDRESS = "0x0000000000000000000000000000000000dEaD";


/* ------------------------------------------------------------------ */
/*  Fonts / design tokens                                              */
/* ------------------------------------------------------------------ */
const FONT_IMPORT = `
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap');
.font-display{font-family:'Space Grotesk',sans-serif;}
.font-body{font-family:'Inter',sans-serif;}
.font-mono{font-family:'IBM Plex Mono',monospace;}
@keyframes growCandle{ from{ transform: scaleY(0); opacity:0;} to{ transform: scaleY(1); opacity:1;} }
@keyframes scanSweep{ 0%{ left:-4%; opacity:0;} 8%{opacity:1;} 92%{opacity:1;} 100%{ left:102%; opacity:0;} }
@keyframes tagFade{ 0%,100%{opacity:0; transform:translateY(4px);} 12%,80%{opacity:1; transform:translateY(0);} }
@keyframes floaty{ 0%,100%{transform:translateY(0px);} 50%{transform:translateY(-6px);} }
@keyframes pulseDot{ 0%,100%{opacity:.35;} 50%{opacity:1;} }
@keyframes glowDrift{ 0%,100%{ transform: translate(-50%,-10%) scale(1); opacity:0.55;} 50%{ transform: translate(-46%,-6%) scale(1.08); opacity:0.8;} }
@keyframes gridPan{ from{ background-position: 0 0, 0 0; } to{ background-position: 44px 44px, 44px 44px; } }
.no-scrollbar::-webkit-scrollbar{display:none;}
.no-scrollbar{-ms-overflow-style:none; scrollbar-width:none;}
`;

const C = {
  ink: "#0B0E14",
  surface: "#12161F",
  surface2: "#171D2B",
  border: "#232B3D",
  gold: "#E8A33D",
  bull: "#3FB68B",
  bear: "#E15554",
  text: "#EDEFF3",
  muted: "#8A93A6",
};

/* ------------------------------------------------------------------ */
/*  Deterministic candle data generator (seeded, no Math.random)       */
/* ------------------------------------------------------------------ */
function mulberry32(seed) {
  return function () {
    seed |= 0;
    seed = (seed + 0x6d2b79f5) | 0;
    let t = Math.imul(seed ^ (seed >>> 15), 1 | seed);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}

function generateCandles(seed = 7, count = 42) {
  const rand = mulberry32(seed);
  let price = 100;
  const candles = [];
  for (let i = 0; i < count; i++) {
    // gentle uptrend with a mid-sequence pullback to simulate a support retest
    const trendBias = i < 24 ? 0.55 : i < 30 ? -0.35 : 0.65;
    const move = (rand() - 0.45 + trendBias * 0.5) * 3.2;
    const open = price;
    price = Math.max(40, price + move);
    const close = price;
    const high = Math.max(open, close) + rand() * 1.6;
    const low = Math.min(open, close) - rand() * 1.6;
    candles.push({ open, close, high, low });
  }
  return candles;
}

/* ------------------------------------------------------------------ */
/*  In-artifact Claude API helper                                      */
/* ------------------------------------------------------------------ */
async function askClaude({ system, messages, imageBase64, imageMediaType }) {
  const content = [];
  if (imageBase64) {
    content.push({
      type: "image",
      source: { type: "base64", media_type: imageMediaType || "image/png", data: imageBase64 },
    });
  }
  const lastUserText = messages[messages.length - 1]?.content || "";
  content.push({ type: "text", text: lastUserText });

  const fullMessages = [
    ...messages.slice(0, -1).map((m) => ({ role: m.role, content: m.content })),
    { role: "user", content },
  ];

  const response = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-6",
      max_tokens: 1000,
      system,
      messages: fullMessages,
    }),
  });
  const data = await response.json();
  const text = (data.content || [])
    .map((b) => (b.type === "text" ? b.text : ""))
    .filter(Boolean)
    .join("\n");
  return text;
}

/* ------------------------------------------------------------------ */
/*  Small shared UI atoms                                              */
/* ------------------------------------------------------------------ */
function Eyebrow({ children }) {
  return (
    <div
      className="font-mono text-[11px] tracking-[0.25em] uppercase inline-flex items-center gap-2 mb-4"
      style={{ color: C.gold }}
    >
      <span className="w-4 h-px" style={{ background: C.gold }} />
      {children}
    </div>
  );
}

function SectionTitle({ eyebrow, title, sub }) {
  return (
    <div className="max-w-2xl mb-12">
      {eyebrow && <Eyebrow>{eyebrow}</Eyebrow>}
      <h2 className="font-display text-3xl md:text-4xl font-semibold" style={{ color: C.text }}>
        {title}
      </h2>
      {sub && (
        <p className="font-body mt-4 text-[15px] leading-relaxed" style={{ color: C.muted }}>
          {sub}
        </p>
      )}
    </div>
  );
}

function Pill({ active, onClick, children }) {
  return (
    <button
      onClick={onClick}
      className="font-mono text-xs tracking-wide px-3.5 py-2 rounded-full border transition-colors"
      style={{
        borderColor: active ? C.gold : C.border,
        color: active ? C.gold : C.muted,
        background: active ? "rgba(232,163,61,0.08)" : "transparent",
      }}
    >
      {children}
    </button>
  );
}

/* ------------------------------------------------------------------ */
/*  Hero: animated candlestick chart (the signature element)           */
/* ------------------------------------------------------------------ */
function HeroChart() {
  const candles = useMemo(() => generateCandles(11, 42), []);
  const maxP = Math.max(...candles.map((c) => c.high));
  const minP = Math.min(...candles.map((c) => c.low));
  const range = maxP - minP;
  const W = 800,
    H = 300,
    pad = 16;
  const cw = (W - pad * 2) / candles.length;

  const yFor = (p) => pad + (1 - (p - minP) / range) * (H - pad * 2);

  const annotations = [
    { label: "Liquidity zone", idx: 8, side: "high", color: C.gold },
    { label: "Support", idx: 27, side: "low", color: C.bull },
    { label: "Resistance", idx: 37, side: "high", color: C.bear },
  ];

  return (
    <div
      className="relative w-full rounded-2xl overflow-hidden border"
      style={{ borderColor: C.border, background: "linear-gradient(180deg, #10141D 0%, #0C0F16 100%)" }}
    >
      <div className="absolute top-4 left-5 flex items-center gap-2 z-10">
        <span
          className="w-1.5 h-1.5 rounded-full"
          style={{ background: C.gold, animation: "pulseDot 1.6s ease-in-out infinite" }}
        />
        <span className="font-mono text-[10px] tracking-[0.2em] uppercase" style={{ color: C.muted }}>
          Arc reading BTC/USD · 4H
        </span>
      </div>

      <svg viewBox={`0 0 ${W} ${H}`} className="w-full h-[260px] md:h-[320px]">
        {[0.2, 0.4, 0.6, 0.8].map((f) => (
          <line key={f} x1={0} x2={W} y1={H * f} y2={H * f} stroke={C.border} strokeWidth="1" />
        ))}
        {candles.map((c, i) => {
          const x = pad + i * cw;
          const isUp = c.close >= c.open;
          const color = isUp ? C.bull : C.bear;
          const bodyTop = yFor(Math.max(c.open, c.close));
          const bodyBottom = yFor(Math.min(c.open, c.close));
          const bodyH = Math.max(1.5, bodyBottom - bodyTop);
          return (
            <g
              key={i}
              style={{
                transformOrigin: `${x + cw / 2}px ${H}px`,
                animation: `growCandle 0.5s ease-out both`,
                animationDelay: `${i * 0.035}s`,
              }}
            >
              <line
                x1={x + cw / 2}
                x2={x + cw / 2}
                y1={yFor(c.high)}
                y2={yFor(c.low)}
                stroke={color}
                strokeWidth="1"
                opacity="0.85"
              />
              <rect
                x={x + cw * 0.18}
                y={bodyTop}
                width={cw * 0.64}
                height={bodyH}
                fill={color}
                opacity="0.9"
                rx="0.6"
              />
            </g>
          );
        })}
      </svg>

      {/* scanning sweep */}
      <div
        className="absolute top-0 bottom-0 w-[6%] pointer-events-none"
        style={{
          background: "linear-gradient(90deg, transparent, rgba(232,163,61,0.16), transparent)",
          animation: "scanSweep 7s linear infinite",
        }}
      />

      {/* annotation tags */}
      {annotations.map((a, i) => {
        const c = candles[a.idx];
        const px = ((pad + a.idx * cw + cw / 2) / W) * 100;
        const py = (yFor(a.side === "high" ? c.high : c.low) / H) * 100;
        return (
          <div
            key={a.label}
            className="absolute font-mono text-[10px] tracking-wide px-2 py-1 rounded-md border whitespace-nowrap"
            style={{
              left: `${px}%`,
              top: `${py}%`,
              transform: "translate(-50%, -140%)",
              borderColor: a.color,
              color: a.color,
              background: "rgba(11,14,20,0.85)",
              animation: `tagFade 7s ease-in-out infinite, floaty 3s ease-in-out infinite`,
              animationDelay: `${i * 0.4}s, ${i * 0.3}s`,
            }}
          >
            {a.label}
          </div>
        );
      })}
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  Wallet / payment mock modal                                        */
/* ------------------------------------------------------------------ */
function DemoModal({ open, onClose, title, body }) {
  if (!open) return null;
  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center p-4" style={{ background: "rgba(6,8,12,0.7)" }}>
      <div
        className="w-full max-w-sm rounded-2xl border p-6 relative"
        style={{ background: C.surface, borderColor: C.border }}
      >
        <button onClick={onClose} className="absolute top-4 right-4" style={{ color: C.muted }}>
          <X size={16} />
        </button>
        <div
          className="w-9 h-9 rounded-lg flex items-center justify-center mb-4"
          style={{ background: "rgba(232,163,61,0.1)" }}
        >
          <Wallet size={16} style={{ color: C.gold }} />
        </div>
        <h3 className="font-display text-lg font-semibold mb-2" style={{ color: C.text }}>
          {title}
        </h3>
        <p className="font-body text-sm leading-relaxed" style={{ color: C.muted }}>
          {body}
        </p>
        <button
          onClick={onClose}
          className="mt-6 w-full font-mono text-xs tracking-wide py-2.5 rounded-lg border"
          style={{ borderColor: C.border, color: C.text }}
        >
          Got it
        </button>
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  How It Works                                                       */
/* ------------------------------------------------------------------ */
function HowItWorks({ wallet, onConnect, onPay }) {
  const [payState, setPayState] = useState("idle"); // idle | paying | done | error
  const [payMsg, setPayMsg] = useState("");

  const steps = [
    {
      n: "01",
      title: "Connect your wallet",
      body: "Link a Web3 wallet to verify ownership and unlock account access.",
      icon: Wallet,
    },
    {
      n: "02",
      title: "Pay in USDC on Arc",
      body: "USDC is Arc's native gas token, sent here through USDC's standard ERC-20 interface — Arc's own recommended integration path — reaching finality in under a second.",
      icon: ShieldCheck,
    },
    {
      n: "03",
      title: "Unlock premium education",
      body: "Get the ARC Mentor, Chart Analyzer, lessons, and journal instantly.",
      icon: Sparkles,
    },
  ];

  const handlePay = async () => {
    setPayState("paying");
    setPayMsg("Confirm in your wallet…");
    try {
      const result = await onPay("5"); // demo amount in USDC
      setPayState(result.status === "confirmed" ? "done" : "error");
      setPayMsg(
        result.status === "confirmed"
          ? `Paid — tx ${shortenAddress(result.txHash)}`
          : `Transaction ${result.status}`
      );
    } catch (err) {
      setPayState("error");
      setPayMsg(err?.message || "Payment failed or was rejected.");
    }
  };

  return (
    <section id="how" className="px-6 md:px-12 py-24 border-t" style={{ borderColor: C.border }}>
      <SectionTitle
        eyebrow="Getting started"
        title="A few steps between you and structured, ARC-guided practice"
      />
      <div className="grid md:grid-cols-3 gap-6">
        {steps.map((s) => (
          <div key={s.n} className="rounded-xl border p-5" style={{ borderColor: C.border, background: C.surface }}>
            <div className="flex items-center justify-between mb-6">
              <span className="font-mono text-xs" style={{ color: C.gold }}>{s.n}</span>
              <s.icon size={16} style={{ color: C.muted }} />
            </div>
            <h3 className="font-display text-[15px] font-semibold mb-2" style={{ color: C.text }}>{s.title}</h3>
            <p className="font-body text-[13px] leading-relaxed" style={{ color: C.muted }}>{s.body}</p>

            {s.n === "01" && !wallet.connected && (
              <button
                onClick={onConnect}
                disabled={wallet.connecting}
                className="mt-4 font-mono text-[11px] tracking-wide px-3 py-2 rounded-lg border inline-flex items-center gap-1.5 disabled:opacity-40"
                style={{ borderColor: C.gold, color: C.gold }}
              >
                {wallet.connecting ? "Connecting…" : "Connect wallet"} <ArrowRight size={11} />
              </button>
            )}
            {s.n === "01" && wallet.connected && (
              <div className="mt-4 font-mono text-[11px] flex items-center gap-1.5" style={{ color: C.bull }}>
                <Check size={12} /> Connected — {shortenAddress(wallet.address)}
              </div>
            )}

            {s.n === "02" && (
              <>
                <button
                  onClick={handlePay}
                  disabled={!wallet.connected || payState === "paying"}
                  className="mt-4 font-mono text-[11px] tracking-wide px-3 py-2 rounded-lg border inline-flex items-center gap-1.5 disabled:opacity-40"
                  style={{ borderColor: C.gold, color: C.gold }}
                >
                  {payState === "paying" ? "Paying…" : "Pay 5 USDC"} <ArrowRight size={11} />
                </button>
                {!wallet.connected && (
                  <div className="mt-2 font-mono text-[10px]" style={{ color: C.muted }}>
                    Connect your wallet first
                  </div>
                )}
                {payMsg && (
                  <div
                    className="mt-2 font-mono text-[10px] break-all"
                    style={{ color: payState === "error" ? C.bear : C.bull }}
                  >
                    {payMsg}
                  </div>
                )}
              </>
            )}
          </div>
        ))}
      </div>
    </section>
  );
}

/* ------------------------------------------------------------------ */
/*  Feature: ARC Chart Analyzer                                         */
/* ------------------------------------------------------------------ */
const ANALYZER_FIELDS = [
  ["marketStructure", "Market structure"],
  ["supportResistance", "Support & resistance"],
  ["trendDirection", "Trend direction"],
  ["liquidityZones", "Liquidity zones"],
  ["entryIdeas", "Entry ideas"],
  ["stopLoss", "Stop loss placement"],
  ["riskReward", "Risk / reward"],
  ["psychology", "Trading psychology"],
];

function ChartAnalyzer() {
  const [image, setImage] = useState(null); // {base64, mediaType, previewUrl}
  const [loading, setLoading] = useState(false);
  const [result, setResult] = useState(null);
  const [error, setError] = useState(null);
  const fileRef = useRef(null);

  const handleFile = (file) => {
    if (!file) return;
    const reader = new FileReader();
    reader.onload = () => {
      const dataUrl = reader.result;
      const base64 = dataUrl.split(",")[1];
      setImage({ base64, mediaType: file.type || "image/png", previewUrl: dataUrl });
      setResult(null);
      setError(null);
    };
    reader.readAsDataURL(file);
  };

  const analyze = async () => {
    if (!image) return;
    setLoading(true);
    setError(null);
    try {
      const text = await askClaude({
        system:
          "You are the ARC Chart Analyzer inside Arc Trading Academy, an education product. Study the uploaded chart image and return ONLY a raw JSON object (no markdown fences, no prose outside it) with these exact keys, each a concise educational explanation of 1-2 sentences: marketStructure, supportResistance, trendDirection, liquidityZones, entryIdeas, stopLoss, riskReward, psychology. Be educational, not financial advice. Base every field on what is actually visible in the image.",
        messages: [
          {
            role: "user",
            content: "Analyze this chart and return the JSON object described in the system prompt.",
          },
        ],
        imageBase64: image.base64,
        imageMediaType: image.mediaType,
      });
      const cleaned = text.replace(/```json|```/g, "").trim();
      const parsed = JSON.parse(cleaned);
      setResult(parsed);
    } catch (e) {
      setError("Couldn't parse the analysis. Try a clearer chart screenshot, or try again.");
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="grid md:grid-cols-2 gap-8">
      <div>
        <p className="font-body text-[14px] leading-relaxed mb-5" style={{ color: C.muted }}>
          Upload any chart screenshot. The ARC walks through structure, key levels, and risk — the same way
          a mentor would talk you through a setup.
        </p>
        <div
          onClick={() => fileRef.current?.click()}
          className="rounded-xl border border-dashed p-6 cursor-pointer flex flex-col items-center justify-center text-center h-56"
          style={{ borderColor: C.border, background: C.surface }}
        >
          {image ? (
            <img src={image.previewUrl} alt="Uploaded chart" className="max-h-44 rounded-lg object-contain" />
          ) : (
            <>
              <Upload size={20} style={{ color: C.gold }} className="mb-3" />
              <span className="font-mono text-xs" style={{ color: C.muted }}>
                Click to upload a chart image
              </span>
            </>
          )}
        </div>
        <input
          ref={fileRef}
          type="file"
          accept="image/*"
          className="hidden"
          onChange={(e) => handleFile(e.target.files?.[0])}
        />
        <button
          onClick={analyze}
          disabled={!image || loading}
          className="mt-4 w-full font-mono text-xs tracking-wide py-3 rounded-lg border flex items-center justify-center gap-2 disabled:opacity-40"
          style={{ borderColor: C.gold, color: C.gold }}
        >
          {loading ? <Loader2 size={14} className="animate-spin" /> : <Sparkles size={14} />}
          {loading ? "Reading the chart…" : "Analyze chart"}
        </button>
        {error && <p className="font-body text-xs mt-3" style={{ color: C.bear }}>{error}</p>}
      </div>

      <div className="space-y-3 max-h-[420px] overflow-y-auto no-scrollbar pr-1">
        {!result && !loading && (
          <div className="h-full min-h-[300px] flex items-center justify-center rounded-xl border" style={{ borderColor: C.border, background: C.surface }}>
            <span className="font-mono text-xs text-center px-6" style={{ color: C.muted }}>
              Your breakdown appears here — market structure, levels, entries, and risk.
            </span>
          </div>
        )}
        {ANALYZER_FIELDS.map(([key, label]) =>
          result?.[key] ? (
            <div key={key} className="rounded-xl border p-4" style={{ borderColor: C.border, background: C.surface }}>
              <div className="font-mono text-[10px] tracking-[0.2em] uppercase mb-1.5" style={{ color: C.gold }}>
                {label}
              </div>
              <p className="font-body text-[13px] leading-relaxed" style={{ color: C.text }}>{result[key]}</p>
            </div>
          ) : null
        )}
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  Feature: ARC Mentor chat                                            */
/* ------------------------------------------------------------------ */
function AIMentor() {
  const quickQuestions = [
    "Why did this trade fail?",
    "Is this trend bullish?",
    "Where is the best entry?",
    "How should I manage risk?",
    "Which timeframe is strongest?",
  ];
  const [messages, setMessages] = useState([
    { role: "assistant", content: "I'm your ARC Mentor. Ask me about market structure, entries, risk, or psychology — I'll explain the reasoning, not just give you an answer." },
  ]);
  const [input, setInput] = useState("");
  const [loading, setLoading] = useState(false);
  const scrollRef = useRef(null);

  useEffect(() => {
    scrollRef.current?.scrollTo({ top: scrollRef.current.scrollHeight, behavior: "smooth" });
  }, [messages, loading]);

  const send = async (text) => {
    const question = (text ?? input).trim();
    if (!question || loading) return;
    const next = [...messages, { role: "user", content: question }];
    setMessages(next);
    setInput("");
    setLoading(true);
    try {
      const reply = await askClaude({
        system:
          "You are the ARC Mentor inside Arc Trading Academy. Give clear, educational explanations about market structure, technical analysis, and risk management. You are teaching concepts, not giving financial advice or specific buy/sell signals — say so briefly if asked for a live call. Keep answers under 120 words, plain language, no markdown headers.",
        messages: next,
      });
      setMessages((m) => [...m, { role: "assistant", content: reply || "Sorry, I couldn't generate a response — try again." }]);
    } catch {
      setMessages((m) => [...m, { role: "assistant", content: "Something went wrong reaching the ARC Mentor. Please try again." }]);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="rounded-2xl border flex flex-col h-[520px]" style={{ borderColor: C.border, background: C.surface }}>
      <div className="flex items-center gap-2 px-5 py-4 border-b" style={{ borderColor: C.border }}>
        <MessageCircle size={15} style={{ color: C.gold }} />
        <span className="font-mono text-xs tracking-wide" style={{ color: C.muted }}>ARC Mentor</span>
      </div>

      <div ref={scrollRef} className="flex-1 overflow-y-auto no-scrollbar px-5 py-4 space-y-3">
        {messages.map((m, i) => (
          <div key={i} className={`flex ${m.role === "user" ? "justify-end" : "justify-start"}`}>
            <div
              className="max-w-[85%] rounded-xl px-3.5 py-2.5 font-body text-[13px] leading-relaxed"
              style={{
                background: m.role === "user" ? "rgba(232,163,61,0.12)" : C.surface2,
                color: C.text,
                border: `1px solid ${m.role === "user" ? "rgba(232,163,61,0.25)" : C.border}`,
              }}
            >
              {m.content}
            </div>
          </div>
        ))}
        {loading && (
          <div className="flex items-center gap-2 font-mono text-[11px]" style={{ color: C.muted }}>
            <Loader2 size={12} className="animate-spin" /> thinking…
          </div>
        )}
      </div>

      <div className="px-5 pb-3 flex gap-2 overflow-x-auto no-scrollbar">
        {quickQuestions.map((q) => (
          <button
            key={q}
            onClick={() => send(q)}
            className="font-mono text-[11px] whitespace-nowrap px-3 py-1.5 rounded-full border"
            style={{ borderColor: C.border, color: C.muted }}
          >
            {q}
          </button>
        ))}
      </div>

      <div className="flex items-center gap-2 px-5 py-4 border-t" style={{ borderColor: C.border }}>
        <input
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyDown={(e) => e.key === "Enter" && send()}
          placeholder="Ask about structure, entries, risk…"
          className="flex-1 bg-transparent outline-none font-body text-[13px]"
          style={{ color: C.text }}
        />
        <button onClick={() => send()} disabled={loading} className="p-2 rounded-lg" style={{ color: C.gold }}>
          <Send size={16} />
        </button>
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  Feature: Interactive Lessons                                       */
/* ------------------------------------------------------------------ */
function Lessons() {
  const [level, setLevel] = useState("Beginner");
  const topics = {
    Beginner: ["Candlestick Patterns", "Market Structure", "Risk Management", "Trading Psychology"],
    Intermediate: ["Smart Money Concepts", "Order Blocks", "Fair Value Gaps", "Fibonacci"],
    Advanced: ["ICT Concepts", "Liquidity", "Multi-timeframe Confluence", "Advanced Risk Models"],
  };
  return (
    <div>
      <div className="flex gap-2 mb-6">
        {Object.keys(topics).map((lvl) => (
          <Pill key={lvl} active={level === lvl} onClick={() => setLevel(lvl)}>{lvl}</Pill>
        ))}
      </div>
      <div className="grid sm:grid-cols-2 gap-3">
        {topics[level].map((t, i) => (
          <div
            key={t}
            className="rounded-xl border p-4 flex items-center justify-between group cursor-default"
            style={{ borderColor: C.border, background: C.surface }}
          >
            <div className="flex items-center gap-3">
              <span className="font-mono text-[10px]" style={{ color: C.muted }}>{String(i + 1).padStart(2, "0")}</span>
              <span className="font-body text-[13px]" style={{ color: C.text }}>{t}</span>
            </div>
            <ChevronRight size={14} style={{ color: C.muted }} />
          </div>
        ))}
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  Feature: Practice Mode                                             */
/* ------------------------------------------------------------------ */
function PracticeMode() {
  const [round, setRound] = useState(0);
  const candles = useMemo(() => generateCandles(19 + round, 32), [round]);
  const [choice, setChoice] = useState(null);
  const [feedback, setFeedback] = useState("");
  const [loading, setLoading] = useState(false);

  const maxP = Math.max(...candles.map((c) => c.high));
  const minP = Math.min(...candles.map((c) => c.low));
  const netMove = candles[candles.length - 1].close - candles[0].open;
  const trendDesc = netMove > 0 ? "an overall uptrend with a mid-sequence pullback" : "a choppy range that leans lower";

  const trade = async (action) => {
    setChoice(action);
    setLoading(true);
    setFeedback("");
    try {
      const reply = await askClaude({
        system:
          "You are reviewing a simulated practice trade inside Arc Trading Academy. Given the described price action and the student's action, explain in under 100 words: what they did correctly, what they may have missed, and one concrete way to improve. Be encouraging but honest. No markdown headers.",
        messages: [
          {
            role: "user",
            content: `Simulated chart shows ${trendDesc}, price ranging between ${minP.toFixed(1)} and ${maxP.toFixed(1)}, closing near ${candles[candles.length - 1].close.toFixed(1)}. The student chose to ${action}. Review this trade.`,
          },
        ],
      });
      setFeedback(reply);
    } catch {
      setFeedback("Couldn't generate feedback right now — try again.");
    } finally {
      setLoading(false);
    }
  };

  const reset = () => {
    setRound((r) => r + 1);
    setChoice(null);
    setFeedback("");
  };

  return (
    <div className="grid md:grid-cols-2 gap-8">
      <div>
        <div className="rounded-xl border overflow-hidden mb-4" style={{ borderColor: C.border, background: C.surface }}>
          <svg viewBox="0 0 640 220" className="w-full h-52">
            {candles.map((c, i) => {
              const cw = 640 / candles.length;
              const x = i * cw;
              const isUp = c.close >= c.open;
              const color = isUp ? C.bull : C.bear;
              const yFor = (p) => 16 + (1 - (p - minP) / (maxP - minP)) * (220 - 32);
              const top = yFor(Math.max(c.open, c.close));
              const bottom = yFor(Math.min(c.open, c.close));
              return (
                <g key={i}>
                  <line x1={x + cw / 2} x2={x + cw / 2} y1={yFor(c.high)} y2={yFor(c.low)} stroke={color} strokeWidth="1" opacity="0.85" />
                  <rect x={x + cw * 0.18} y={top} width={cw * 0.64} height={Math.max(1.5, bottom - top)} fill={color} opacity="0.9" />
                </g>
              );
            })}
          </svg>
        </div>
        <div className="flex gap-3">
          <button
            onClick={() => trade("buy")}
            disabled={loading}
            className="flex-1 font-mono text-xs tracking-wide py-3 rounded-lg border flex items-center justify-center gap-2"
            style={{ borderColor: C.bull, color: C.bull, opacity: choice && choice !== "buy" ? 0.4 : 1 }}
          >
            <TrendingUp size={14} /> Buy
          </button>
          <button
            onClick={() => trade("sell")}
            disabled={loading}
            className="flex-1 font-mono text-xs tracking-wide py-3 rounded-lg border flex items-center justify-center gap-2"
            style={{ borderColor: C.bear, color: C.bear, opacity: choice && choice !== "sell" ? 0.4 : 1 }}
          >
            <TrendingDown size={14} /> Sell
          </button>
        </div>
        {choice && (
          <button onClick={reset} className="mt-3 font-mono text-[11px]" style={{ color: C.muted }}>
            New round →
          </button>
        )}
      </div>

      <div className="rounded-xl border p-5 min-h-[220px]" style={{ borderColor: C.border, background: C.surface }}>
        <div className="font-mono text-[10px] tracking-[0.2em] uppercase mb-3" style={{ color: C.gold }}>ARC review</div>
        {!choice && (
          <p className="font-body text-[13px]" style={{ color: C.muted }}>
            Take a position on the chart to get a personalized breakdown of what you did well and what to
            watch next time.
          </p>
        )}
        {loading && (
          <div className="flex items-center gap-2 font-mono text-[11px]" style={{ color: C.muted }}>
            <Loader2 size={12} className="animate-spin" /> reviewing your trade…
          </div>
        )}
        {feedback && <p className="font-body text-[13px] leading-relaxed" style={{ color: C.text }}>{feedback}</p>}
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  Feature: Trading Journal (persisted via window.storage)            */
/* ------------------------------------------------------------------ */
function Journal() {
  const [entries, setEntries] = useState([]);
  const [loaded, setLoaded] = useState(false);
  const [form, setForm] = useState({ result: "win", emotion: "Calm", notes: "" });

  useEffect(() => {
    (async () => {
      try {
        const res = await window.storage?.get?.("journal:entries");
        if (res?.value) setEntries(JSON.parse(res.value));
      } catch {
        // no existing entries yet
      } finally {
        setLoaded(true);
      }
    })();
  }, []);

  const save = async (next) => {
    setEntries(next);
    try {
      await window.storage?.set?.("journal:entries", JSON.stringify(next));
    } catch {
      // storage unavailable — state still updates for this session
    }
  };

  const addEntry = () => {
    if (!form.notes.trim()) return;
    const entry = { ...form, id: Date.now(), date: new Date().toLocaleDateString() };
    save([entry, ...entries]);
    setForm({ result: "win", emotion: "Calm", notes: "" });
  };

  const winRate = entries.length
    ? Math.round((entries.filter((e) => e.result === "win").length / entries.length) * 100)
    : 0;

  return (
    <div className="grid md:grid-cols-[1fr_1.2fr] gap-8">
      <div>
        <div className="rounded-xl border p-5 mb-4" style={{ borderColor: C.border, background: C.surface }}>
          <div className="flex items-center justify-between mb-1">
            <span className="font-mono text-[10px] tracking-[0.2em] uppercase" style={{ color: C.muted }}>Win rate</span>
            <Percent size={13} style={{ color: C.gold }} />
          </div>
          <div className="font-display text-3xl font-semibold" style={{ color: C.text }}>{winRate}%</div>
          <div className="w-full h-1.5 rounded-full mt-3" style={{ background: C.border }}>
            <div className="h-1.5 rounded-full" style={{ width: `${winRate}%`, background: C.bull }} />
          </div>
          <span className="font-mono text-[11px]" style={{ color: C.muted }}>{entries.length} logged trades</span>
        </div>

        <div className="rounded-xl border p-5 space-y-3" style={{ borderColor: C.border, background: C.surface }}>
          <div className="flex gap-2">
            {["win", "loss"].map((r) => (
              <Pill key={r} active={form.result === r} onClick={() => setForm((f) => ({ ...f, result: r }))}>
                {r}
              </Pill>
            ))}
          </div>
          <select
            value={form.emotion}
            onChange={(e) => setForm((f) => ({ ...f, emotion: e.target.value }))}
            className="w-full bg-transparent border rounded-lg px-3 py-2 font-body text-[13px] outline-none"
            style={{ borderColor: C.border, color: C.text }}
          >
            {["Calm", "Confident", "FOMO", "Revenge", "Hesitant"].map((e) => (
              <option key={e} value={e} style={{ background: C.surface }}>{e}</option>
            ))}
          </select>
          <textarea
            value={form.notes}
            onChange={(e) => setForm((f) => ({ ...f, notes: e.target.value }))}
            placeholder="What happened, and what will you do differently?"
            rows={3}
            className="w-full bg-transparent border rounded-lg px-3 py-2 font-body text-[13px] outline-none resize-none"
            style={{ borderColor: C.border, color: C.text }}
          />
          <button
            onClick={addEntry}
            className="w-full font-mono text-xs tracking-wide py-2.5 rounded-lg border"
            style={{ borderColor: C.gold, color: C.gold }}
          >
            Log entry
          </button>
        </div>
      </div>

      <div className="space-y-3 max-h-[440px] overflow-y-auto no-scrollbar pr-1">
        {!loaded && <span className="font-mono text-xs" style={{ color: C.muted }}>Loading journal…</span>}
        {loaded && entries.length === 0 && (
          <div className="h-full min-h-[200px] flex items-center justify-center rounded-xl border" style={{ borderColor: C.border, background: C.surface }}>
            <span className="font-mono text-xs text-center px-6" style={{ color: C.muted }}>
              No entries yet — your logged trades stay saved here.
            </span>
          </div>
        )}
        {entries.map((e) => (
          <div key={e.id} className="rounded-xl border p-4" style={{ borderColor: C.border, background: C.surface }}>
            <div className="flex items-center justify-between mb-2">
              <span
                className="font-mono text-[10px] tracking-wide uppercase px-2 py-0.5 rounded-full"
                style={{
                  color: e.result === "win" ? C.bull : C.bear,
                  background: e.result === "win" ? "rgba(63,182,139,0.1)" : "rgba(225,85,84,0.1)",
                }}
              >
                {e.result}
              </span>
              <span className="font-mono text-[10px]" style={{ color: C.muted }}>{e.date} · {e.emotion}</span>
            </div>
            <p className="font-body text-[13px]" style={{ color: C.text }}>{e.notes}</p>
          </div>
        ))}
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  Feature: ARC Market Scanner (preview / mock data)                   */
/* ------------------------------------------------------------------ */
function Scanner() {
  const [market, setMarket] = useState("Crypto");
  const data = {
    Crypto: [
      { sym: "BTC/USD", note: "Uptrend, retested support", dir: "up" },
      { sym: "ETH/USD", note: "Consolidating below resistance", dir: "flat" },
      { sym: "SOL/USD", note: "Liquidity sweep, watching reaction", dir: "down" },
    ],
    Forex: [
      { sym: "EUR/USD", note: "Range-bound, structure unclear", dir: "flat" },
      { sym: "GBP/JPY", note: "Breakout above resistance", dir: "up" },
      { sym: "USD/CHF", note: "Lower high forming", dir: "down" },
    ],
    Stocks: [
      { sym: "NVDA", note: "Strong trend, pullback to support", dir: "up" },
      { sym: "TSLA", note: "Choppy, low conviction", dir: "flat" },
      { sym: "AAPL", note: "Rejected at resistance", dir: "down" },
    ],
  };
  const dirColor = { up: C.bull, down: C.bear, flat: C.gold };
  const DirIcon = { up: TrendingUp, down: TrendingDown, flat: ScanLine };

  return (
    <div>
      <div className="flex items-center justify-between mb-6">
        <div className="flex gap-2">
          {Object.keys(data).map((m) => (
            <Pill key={m} active={market === m} onClick={() => setMarket(m)}>{m}</Pill>
          ))}
        </div>
        <span className="font-mono text-[10px] uppercase tracking-wide" style={{ color: C.muted }}>Preview data</span>
      </div>
      <div className="grid sm:grid-cols-3 gap-3">
        {data[market].map((row) => {
          const Icon = DirIcon[row.dir];
          return (
            <div key={row.sym} className="rounded-xl border p-4" style={{ borderColor: C.border, background: C.surface }}>
              <div className="flex items-center justify-between mb-3">
                <span className="font-mono text-sm" style={{ color: C.text }}>{row.sym}</span>
                <Icon size={15} style={{ color: dirColor[row.dir] }} />
              </div>
              <p className="font-body text-[12px] leading-relaxed" style={{ color: C.muted }}>{row.note}</p>
            </div>
          );
        })}
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/*  ARC Features section wrapper (tabs)                                 */
/* ------------------------------------------------------------------ */
function AIFeatures() {
  const tabs = [
    { key: "analyzer", label: "Chart Analyzer", icon: LineChartIcon, render: ChartAnalyzer },
    { key: "mentor", label: "ARC Mentor", icon: Brain, render: AIMentor },
    { key: "lessons", label: "Lessons", icon: BookOpen, render: Lessons },
    { key: "practice", label: "Practice Mode", icon: Target, render: PracticeMode },
    { key: "journal", label: "Journal", icon: NotebookPen, render: Journal },
    { key: "scanner", label: "Market Scanner", icon: Radar, render: Scanner },
  ];
  const [active, setActive] = useState("analyzer");
  const Active = tabs.find((t) => t.key === active).render;

  return (
    <section id="features" className="px-6 md:px-12 py-24 border-t" style={{ borderColor: C.border }}>
      <SectionTitle
        eyebrow="ARC features"
        title="Every feature is a way to practice reading the market"
        sub="Switch between the tools — each one is wired to explain its reasoning, not just hand you an answer."
      />
      <div className="flex gap-2 mb-8 overflow-x-auto no-scrollbar">
        {tabs.map((t) => (
          <button
            key={t.key}
            onClick={() => setActive(t.key)}
            className="flex items-center gap-2 font-mono text-xs tracking-wide px-4 py-2.5 rounded-lg border whitespace-nowrap transition-colors"
            style={{
              borderColor: active === t.key ? C.gold : C.border,
              color: active === t.key ? C.gold : C.muted,
              background: active === t.key ? "rgba(232,163,61,0.06)" : "transparent",
            }}
          >
            <t.icon size={13} /> {t.label}
          </button>
        ))}
      </div>
      <Active />
    </section>
  );
}

/* ------------------------------------------------------------------ */
/*  Tech stack                                                         */
/* ------------------------------------------------------------------ */
function TechStack() {
  const stack = [
    ["Frontend", "Next.js + Tailwind CSS"],
    ["Backend", "Node.js + Express"],
    ["Database", "PostgreSQL / Supabase"],
    ["Auth", "WalletConnect / RainbowKit"],
    ["Payments", "USDC on Arc Network"],
    ["ARC", "OpenAI API"],
    ["Charts", "TradingView Advanced Charts"],
    ["Hosting", "Vercel"],
    ["Storage", "Cloudinary / Supabase"],
  ];
  return (
    <section className="px-6 md:px-12 py-24 border-t" style={{ borderColor: C.border }}>
      <SectionTitle eyebrow="Under the hood" title="Built for a production Web3 education product" />
      <div className="grid sm:grid-cols-3 md:grid-cols-3 gap-3">
        {stack.map(([label, val]) => (
          <div key={label} className="rounded-xl border p-4 flex items-center justify-between" style={{ borderColor: C.border, background: C.surface }}>
            <span className="font-mono text-[11px] uppercase tracking-wide" style={{ color: C.muted }}>{label}</span>
            <span className="font-body text-[13px] text-right" style={{ color: C.text }}>{val}</span>
          </div>
        ))}
      </div>
    </section>
  );
}

/* ------------------------------------------------------------------ */
/*  Root                                                                */
/* ------------------------------------------------------------------ */
export default function App() {
  const [modal, setModal] = useState({ open: false, title: "", body: "" });
  const openDemo = (title, body) => setModal({ open: true, title, body });

  const [wallet, setWallet] = useState({ connected: false, connecting: false, address: null, balance: null, error: null });

  const scrollTo = (id) => document.getElementById(id)?.scrollIntoView({ behavior: "smooth" });

  const handleConnect = async () => {
    setWallet((w) => ({ ...w, connecting: true, error: null }));
    try {
      const { address, balance } = await connectArcWallet();
      setWallet({ connected: true, connecting: false, address, balance, error: null });
    } catch (err) {
      setWallet((w) => ({ ...w, connecting: false, error: err?.message || "Failed to connect wallet." }));
      openDemo("Couldn't connect", err?.message || "Failed to connect a wallet. Make sure a browser wallet extension is installed.");
    }
  };

  const handlePay = async (amountUsdc) => {
    if (!wallet.connected || !wallet.address) throw new Error("Connect your wallet first.");
    const result = await payWithUsdc(wallet.address, MERCHANT_ADDRESS, amountUsdc);
    const balance = await refreshArcBalance(wallet.address);
    setWallet((w) => ({ ...w, balance }));
    return result;
  };

  return (
    <div className="min-h-screen font-body relative" style={{ background: C.ink, color: C.text }}>
      <style>{FONT_IMPORT}</style>

      {/* technical background: fine grid + scanlines + drifting glow */}
      <div className="fixed inset-0 pointer-events-none" style={{ zIndex: 0 }}>
        <div
          className="absolute inset-0"
          style={{
            backgroundImage: `
              linear-gradient(rgba(232,163,61,0.05) 1px, transparent 1px),
              linear-gradient(90deg, rgba(232,163,61,0.05) 1px, transparent 1px)
            `,
            backgroundSize: "44px 44px, 44px 44px",
            animation: "gridPan 26s linear infinite",
            maskImage: "radial-gradient(ellipse 80% 60% at 50% 0%, black 0%, transparent 75%)",
            WebkitMaskImage: "radial-gradient(ellipse 80% 60% at 50% 0%, black 0%, transparent 75%)",
          }}
        />
        <div
          className="absolute inset-0"
          style={{
            backgroundImage:
              "repeating-linear-gradient(0deg, rgba(255,255,255,0.02) 0px, rgba(255,255,255,0.02) 1px, transparent 1px, transparent 3px)",
          }}
        />
        <div
          className="absolute w-[900px] h-[520px] rounded-full"
          style={{
            left: "50%",
            top: 0,
            background: "radial-gradient(ellipse, rgba(232,163,61,0.14), transparent 65%)",
            filter: "blur(10px)",
            animation: "glowDrift 14s ease-in-out infinite",
          }}
        />
      </div>

      <div className="relative" style={{ zIndex: 1 }}>

      {/* Nav */}
      <header className="sticky top-0 z-40 backdrop-blur border-b" style={{ borderColor: C.border, background: "rgba(11,14,20,0.75)" }}>
        <div className="px-6 md:px-12 h-16 flex items-center justify-between">
          <div className="flex items-center gap-2">
            <ScanLine size={16} style={{ color: C.gold }} />
            <span className="font-display text-sm font-semibold tracking-wide">Arc Trading Academy</span>
            <span className="hidden sm:inline font-mono text-[10px] tracking-wide ml-1" style={{ color: C.muted }}>
              arctradingacademy.com
            </span>
          </div>
          <nav className="hidden md:flex items-center gap-8 font-mono text-xs" style={{ color: C.muted }}>
            <button onClick={() => scrollTo("how")}>How it works</button>
            <button onClick={() => scrollTo("features")}>ARC features</button>
          </nav>
          <button
            onClick={handleConnect}
            disabled={wallet.connecting}
            className="flex items-center gap-2 font-mono text-xs px-3.5 py-2 rounded-lg border disabled:opacity-50"
            style={{ borderColor: wallet.connected ? C.bull : C.border, color: wallet.connected ? C.bull : C.text }}
          >
            <Wallet size={13} />
            {wallet.connecting
              ? "Connecting…"
              : wallet.connected
              ? `${shortenAddress(wallet.address)} · ${wallet.balance} USDC`
              : "Connect Wallet"}
          </button>
        </div>
      </header>

      {/* Hero */}
      <section className="px-6 md:px-12 pt-16 pb-24 grid md:grid-cols-2 gap-12 items-center">
        <div>
          <Eyebrow>ARC-guided market education</Eyebrow>
          <h1 className="font-display text-4xl md:text-5xl font-semibold leading-[1.1]" style={{ color: C.text }}>
            Learn to read charts like a professional — with ARC.
          </h1>
          <p className="font-body mt-6 text-[15px] leading-relaxed max-w-md" style={{ color: C.muted }}>
            Arc Trading Academy combines artificial intelligence, real-time market analysis, and interactive
            lessons to help traders understand price action, market structure, and risk management. Improve
            your skills — not just your signals.
          </p>
          <div className="flex flex-wrap gap-3 mt-8">
            <button
              onClick={() => scrollTo("features")}
              className="flex items-center gap-2 font-mono text-xs tracking-wide px-5 py-3 rounded-lg"
              style={{ background: C.gold, color: C.ink }}
            >
              Start Learning <ArrowRight size={14} />
            </button>
            <button
              onClick={handleConnect}
              disabled={wallet.connecting}
              className="flex items-center gap-2 font-mono text-xs tracking-wide px-5 py-3 rounded-lg border disabled:opacity-50"
              style={{ borderColor: wallet.connected ? C.bull : C.border, color: wallet.connected ? C.bull : C.text }}
            >
              <Wallet size={14} />
              {wallet.connecting
                ? "Connecting…"
                : wallet.connected
                ? `${shortenAddress(wallet.address)} · ${wallet.balance} USDC`
                : "Connect Wallet"}
            </button>
          </div>
        </div>
        <HeroChart />
      </section>

      <HowItWorks wallet={wallet} onConnect={handleConnect} onPay={handlePay} />
      <AIFeatures />
      <TechStack />

      <footer className="px-6 md:px-12 py-10 border-t flex flex-col md:flex-row items-center justify-between gap-4" style={{ borderColor: C.border }}>
        <span className="font-mono text-[11px]" style={{ color: C.muted }}>© {new Date().getFullYear()} Arc Trading Academy · arctradingacademy.com — education only, not financial advice.</span>
        <div className="flex items-center gap-2">
          <ScanLine size={13} style={{ color: C.gold }} />
          <span className="font-display text-xs" style={{ color: C.muted }}>Built to teach, not to tip.</span>
        </div>
      </footer>

      <DemoModal open={modal.open} onClose={() => setModal((m) => ({ ...m, open: false }))} title={modal.title} body={modal.body} />
      </div>
    </div>
  );
}
