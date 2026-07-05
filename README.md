# test-
import { useState } from "react";

const T = {
  bg: "#F8F4EF",
  card: "#FFFFFF",
  navy: "#1C1B30",
  amber: "#C17A3A",
  muted: "#8A8898",
  border: "#E8E3DD",
  green: "#4A8970",
  red: "#C04848",
  text: "#2A2A3C",
  light: "#F8F4EF",
};

const preguntas = [
  {
    q: "¿Cuántos métodos o dietas intentaste en los últimos 5 años?",
    opts: [
      { t: "Solo 1 o 2 veces", s: 1 },
      { t: "Entre 3 y 5 veces", s: 2 },
      { t: "Más de 5 veces", s: 3 },
      { t: "Perdí la cuenta", s: 4 },
    ],
  },
  {
    q: "¿Cómo es tu nivel de energía en el día a día?",
    opts: [
      { t: "Bien, me mantengo activa sin problemas", s: 1 },
      { t: "Variable — hay días que estoy agotada", s: 2 },
      { t: "Baja en general, me cuesta arrancar", s: 3 },
      { t: "Muy baja, casi siempre me siento sin fuerzas", s: 4 },
    ],
  },
  {
    q: "Cuando lograste bajar de peso… ¿qué pasó después?",
    opts: [
      { t: "Lo mantuve sin demasiado esfuerzo", s: 1 },
      { t: "Lo mantuve un tiempo, pero subí algo", s: 2 },
      { t: "Recuperé casi todo lo que había bajado", s: 3 },
      { t: "Terminé pesando más que antes", s: 4 },
    ],
  },
  {
    q: "¿Cómo describís tu relación emocional con la comida?",
    opts: [
      { t: "Tranquila, como sin culpa", s: 1 },
      { t: "A veces tengo ansiedad, pero lo manejo", s: 2 },
      { t: "El picoteo y la ansiedad son frecuentes", s: 3 },
      { t: "La comida me genera mucha culpa y estrés", s: 4 },
    ],
  },
  {
    q: "Cuando hacés ejercicio, ¿cómo responde tu cuerpo?",
    opts: [
      { t: "Bien, noto cambios en relativamente poco tiempo", s: 1 },
      { t: "Mejoro, pero muy lentamente", s: 2 },
      { t: "Hago ejercicio y casi no veo cambios", s: 3 },
      { t: "Mi cuerpo parece no responder a nada", s: 4 },
    ],
  },
];

const perfiles = {
  bajo: {
    tipo: "Metabolismo Activo",
    dot: "#4A8970",
    desc: "Tu metabolismo todavía responde bien. Estás en una etapa temprana de los cambios hormonales y tenés una ventana de oportunidad real. Con el enfoque correcto ahora, podés construir hábitos que duren para siempre — sin el ciclo de rebote.",
    clave: "Tu momento es ahora, antes de que el patrón se instale.",
  },
  medio: {
    tipo: "Metabolismo Bloqueado",
    dot: "#C17A3A",
    desc: "El historial de dietas y el ciclo de rebote entrenaron a tu metabolismo para resistir el cambio. No es falta de voluntad — es biología. La buena noticia: el metabolismo bloqueado responde muy bien a un protocolo específico de reprogramación.",
    clave: "Lo que necesitás no es otra dieta. Es reprogramación.",
  },
  alto: {
    tipo: "Metabolismo en Ciclo Crónico",
    dot: "#C04848",
    desc: "Años de intentos, restricciones y rebotes crearon un ciclo que se retroalimenta. Tu metabolismo aprendió a defenderse. Esto no es permanente — pero requiere un abordaje completamente diferente a todo lo que ya intentaste.",
    clave: "Cada intento fallido refuerza el ciclo. Hay que romperlo.",
  },
};

const getPerfil = (score) => {
  if (score <= 8) return perfiles.bajo;
  if (score <= 14) return perfiles.medio;
  return perfiles.alto;
};

export default function TestMetabolismo() {
  const [step, setStep] = useState(-1);
  const [answers, setAnswers] = useState([]);
  const [sel, setSel] = useState(null);

  const score = answers.reduce((a, b) => a + b, 0);
  const done = step >= preguntas.length;
  const perfil = done ? getPerfil(score) : null;
  const q = !done && step >= 0 ? preguntas[step] : null;

  const next = () => {
    if (sel === null) return;
    const na = [...answers, sel];
    setAnswers(na);
    setSel(null);
    setStep(step + 1);
  };

  const pct = step >= 0 && !done ? Math.round(((step + 1) / preguntas.length) * 100) : 100;

  /* ─── Intro ─── */
  if (step === -1) return (
    <div style={{ fontFamily: "system-ui, -apple-system, sans-serif", background: T.bg, minHeight: "100vh", display: "flex", alignItems: "center", justifyContent: "center", padding: "24px" }}>
      <div style={{ background: T.card, borderRadius: "24px", padding: "40px 36px", maxWidth: "480px", width: "100%", boxShadow: "0 12px 56px rgba(0,0,0,0.08)" }}>
        <span style={{ display: "inline-block", background: T.amber + "18", color: T.amber, fontSize: "11px", fontWeight: "700", letterSpacing: "0.12em", textTransform: "uppercase", padding: "4px 10px", borderRadius: "6px", marginBottom: "20px" }}>
          SANTIAGO BUSCAGLIA
        </span>
        <h1 style={{ color: T.navy, fontSize: "28px", fontWeight: "800", lineHeight: "1.2", margin: "0 0 12px" }}>
          ¿Qué tipo de metabolismo tenés?
        </h1>
        <p style={{ color: T.muted, fontSize: "15px", lineHeight: "1.6", margin: "0 0 28px" }}>
          Descubrí por qué tu cuerpo no responde como antes y qué tipo de abordaje necesitás realmente.
        </p>
        <div style={{ background: T.light, borderRadius: "14px", padding: "18px 20px", marginBottom: "28px" }}>
          {["Nivel de energía actual", "Historial de métodos y dietas", "Patrón de rebote", "Relación emocional con la comida", "Respuesta al ejercicio"].map((item, i) => (
            <div key={i} style={{ display: "flex", alignItems: "center", gap: "12px", marginBottom: i < 4 ? "10px" : "0" }}>
              <div style={{ width: "5px", height: "5px", borderRadius: "50%", background: T.amber, flexShrink: 0 }} />
              <span style={{ color: T.text, fontSize: "14px" }}>{item}</span>
            </div>
          ))}
        </div>
        <p style={{ color: T.muted, fontSize: "12px", margin: "0 0 16px", textAlign: "center" }}>5 preguntas · 3 minutos · Resultado inmediato</p>
        <button
          onClick={() => setStep(0)}
          style={{ background: T.amber, color: "#fff", border: "none", borderRadius: "14px", padding: "16px", fontSize: "16px", fontWeight: "700", cursor: "pointer", width: "100%", letterSpacing: "-0.01em" }}
        >
          Comenzar el test →
        </button>
      </div>
    </div>
  );

  /* ─── Resultado ─── */
  if (done && perfil) return (
    <div style={{ fontFamily: "system-ui, -apple-system, sans-serif", background: T.bg, minHeight: "100vh", display: "flex", alignItems: "center", justifyContent: "center", padding: "24px" }}>
      <div style={{ background: T.card, borderRadius: "24px", padding: "40px 36px", maxWidth: "480px", width: "100%", boxShadow: "0 12px 56px rgba(0,0,0,0.08)" }}>
        <p style={{ color: T.muted, fontSize: "12px", letterSpacing: "0.1em", textTransform: "uppercase", margin: "0 0 16px" }}>Tu resultado</p>

        {/* Score ring */}
        <div style={{ display: "flex", alignItems: "center", gap: "20px", marginBottom: "24px" }}>
          <div style={{ position: "relative", flexShrink: 0 }}>
            <svg width="80" height="80" viewBox="0 0 80 80">
              <circle cx="40" cy="40" r="32" fill="none" stroke={T.border} strokeWidth="8" />
              <circle
                cx="40" cy="40" r="32"
                fill="none"
                stroke={perfil.dot}
                strokeWidth="8"
                strokeDasharray={`${(score / 20) * 201} 201`}
                strokeLinecap="round"
                transform="rotate(-90 40 40)"
              />
              <text x="40" y="45" textAnchor="middle" fill={T.navy} fontSize="16" fontWeight="800">{score}</text>
            </svg>
          </div>
          <div>
            <div style={{ width: "10px", height: "10px", borderRadius: "50%", background: perfil.dot, display: "inline-block", marginRight: "8px", verticalAlign: "middle" }} />
            <span style={{ color: perfil.dot, fontSize: "18px", fontWeight: "800", verticalAlign: "middle" }}>{perfil.tipo}</span>
            <p style={{ color: T.muted, fontSize: "13px", margin: "4px 0 0" }}>Puntaje {score}/20</p>
          </div>
        </div>

        <p style={{ color: T.text, fontSize: "15px", lineHeight: "1.7", margin: "0 0 16px" }}>{perfil.desc}</p>

        <div style={{ borderLeft: `3px solid ${perfil.dot}`, paddingLeft: "14px", marginBottom: "32px" }}>
          <p style={{ color: perfil.dot, fontSize: "14px", fontWeight: "700", margin: "0" }}>{perfil.clave}</p>
        </div>

        {/* CTA */}
        <div style={{ background: T.navy, borderRadius: "18px", padding: "28px", textAlign: "center" }}>
          <p style={{ color: "#F8F4EF", fontSize: "16px", fontWeight: "700", margin: "0 0 8px", lineHeight: "1.3" }}>
            ¿Querés que Santiago analice tu caso específico?
          </p>
          <p style={{ color: T.muted, fontSize: "13px", margin: "0 0 20px", lineHeight: "1.5" }}>
            Completá el formulario de 3 minutos y recibí un plan personalizado para tu perfil.
          </p>
          <button
            style={{ background: T.amber, color: "#fff", border: "none", borderRadius: "12px", padding: "15px 28px", fontSize: "15px", fontWeight: "700", cursor: "pointer", width: "100%" }}
            onClick={() => alert("Redirigir al formulario Tally aquí")}
          >
            Completar el formulario →
          </button>
        </div>
      </div>
    </div>
  );

  /* ─── Pregunta ─── */
  return (
    <div style={{ fontFamily: "system-ui, -apple-system, sans-serif", background: T.bg, minHeight: "100vh", display: "flex", alignItems: "center", justifyContent: "center", padding: "24px" }}>
      <div style={{ background: T.card, borderRadius: "24px", padding: "36px", maxWidth: "480px", width: "100%", boxShadow: "0 12px 56px rgba(0,0,0,0.08)" }}>
        {/* Progress */}
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: "10px" }}>
          <span style={{ color: T.amber, fontSize: "11px", fontWeight: "700", letterSpacing: "0.12em", textTransform: "uppercase" }}>TEST DEL METABOLISMO</span>
          <span style={{ color: T.muted, fontSize: "12px" }}>{step + 1} / {preguntas.length}</span>
        </div>
        <div style={{ background: T.border, borderRadius: "4px", height: "3px", marginBottom: "32px" }}>
          <div style={{ background: T.amber, borderRadius: "4px", height: "3px", width: `${pct}%`, transition: "width 0.4s ease" }} />
        </div>

        <h2 style={{ color: T.navy, fontSize: "20px", fontWeight: "700", lineHeight: "1.4", margin: "0 0 24px" }}>
          {q.q}
        </h2>

        <div style={{ display: "flex", flexDirection: "column", gap: "10px", marginBottom: "28px" }}>
          {q.opts.map((opt, i) => (
            <button
              key={i}
              onClick={() => setSel(opt.s)}
              style={{
                background: sel === opt.s ? T.amber : T.light,
                color: sel === opt.s ? "#fff" : T.text,
                border: `2px solid ${sel === opt.s ? T.amber : T.border}`,
                borderRadius: "14px",
                padding: "14px 18px",
                fontSize: "14px",
                fontWeight: sel === opt.s ? "600" : "400",
                cursor: "pointer",
                textAlign: "left",
                transition: "all 0.15s ease",
                lineHeight: "1.4",
              }}
            >
              {opt.t}
            </button>
          ))}
        </div>

        <button
          onClick={next}
          disabled={sel === null}
          style={{
            background: sel !== null ? T.navy : T.border,
            color: sel !== null ? "#fff" : T.muted,
            border: "none",
            borderRadius: "14px",
            padding: "16px",
            fontSize: "15px",
            fontWeight: "700",
            cursor: sel !== null ? "pointer" : "not-allowed",
            width: "100%",
            transition: "all 0.2s ease",
          }}
        >
          {step + 1 === preguntas.length ? "Ver mi resultado →" : "Siguiente →"}
        </button>
      </div>
    </div>
  );
}
