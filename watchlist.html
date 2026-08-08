import { useState, useEffect, useCallback, useRef } from "react";

// ─── Data config ───────────────────────────────────────────────────────────
const SERVICES = [
  { id: "netflix",   name: "Netflix",        color: "#E50914" },
  { id: "disney",    name: "Disney+",        color: "#1140D4" },
  { id: "stan",      name: "Stan",           color: "#0077B5" },
  { id: "binge",     name: "Binge",          color: "#FF5500" },
  { id: "paramount", name: "Paramount+",     color: "#0064FF" },
  { id: "appletv",   name: "Apple TV+",      color: "#555" },
  { id: "prime",     name: "Prime Video",    color: "#00A8E0" },
  { id: "iview",     name: "ABC iview",      color: "#FF6600" },
  { id: "sbs",       name: "SBS On Demand",  color: "#CC0000" },
  { id: "9now",      name: "9Now",           color: "#FF8000" },
  { id: "7plus",     name: "7Plus",          color: "#AA0000" },
  { id: "10play",    name: "10 Play",        color: "#006699" },
  { id: "foxtel",    name: "Foxtel Now",     color: "#FF6000" },
  { id: "youtube",   name: "YouTube",        color: "#FF0000" },
  { id: "fta",       name: "Free to Air",    color: "#6B7280" },
  { id: "other",     name: "Other / Custom", color: "#8B5CF6" },
];

const SVC = Object.fromEntries(SERVICES.map(s => [s.id, s]));

const STATUS = {
  watching:  { label: "Watching",       emoji: "▶",  color: "#22C55E", bg: "rgba(34,197,94,0.13)",    border: "rgba(34,197,94,0.35)"    },
  want:      { label: "Want to Watch",  emoji: "🔖", color: "#60A5FA", bg: "rgba(96,165,250,0.13)",   border: "rgba(96,165,250,0.35)"   },
  paused:    { label: "Paused",         emoji: "⏸",  color: "#FBBF24", bg: "rgba(251,191,36,0.13)",   border: "rgba(251,191,36,0.35)"   },
  finished:  { label: "Finished",       emoji: "✓",  color: "#A78BFA", bg: "rgba(167,139,250,0.13)",  border: "rgba(167,139,250,0.35)"  },
};

const MEMBER_COLORS = ["#EF4444","#F97316","#EAB308","#22C55E","#06B6D4","#818CF8","#EC4899","#14B8A6"];

// ─── Local storage helpers ──────────────────────────────────────────────────
const ls = {
  get: k => { try { return JSON.parse(localStorage.getItem(k)); } catch { return null; } },
  set: (k, v) => localStorage.setItem(k, JSON.stringify(v)),
  del: k => localStorage.removeItem(k),
};

// ─── Firebase REST helpers (module-level state) ─────────────────────────────
let FB = { url: "", code: "" };
const fpath = seg => `${FB.url}/${FB.code}/${seg}.json`;
const fGet  = async seg => { const r = await fetch(fpath(seg)); if (!r.ok) throw r; return r.json(); };
const fPost = async (seg, d) => { const r = await fetch(fpath(seg), { method:"POST", headers:{"Content-Type":"application/json"}, body:JSON.stringify(d) }); if (!r.ok) throw r; return r.json(); };
const fPatch = async (seg, d) => { const r = await fetch(fpath(seg), { method:"PATCH", headers:{"Content-Type":"application/json"}, body:JSON.stringify(d) }); if (!r.ok) throw r; };
const fDel  = async seg => fetch(fpath(seg), { method:"DELETE" });

// ─── Shared sub-components ──────────────────────────────────────────────────
function Avatar({ name = "?", color = "#818CF8", size = 34 }) {
  return (
    <div style={{ width:size, height:size, borderRadius:"50%", background:color, flexShrink:0,
      display:"flex", alignItems:"center", justifyContent:"center",
      fontSize:size*0.4, fontWeight:800, color:"#fff", letterSpacing:"-0.5px" }}>
      {name[0]?.toUpperCase()}
    </div>
  );
}

function Badge({ label, color, bg, border, emoji }) {
  return (
    <span style={{ fontSize:11, fontWeight:700, padding:"3px 8px", borderRadius:100,
      background:bg, color, border:`1px solid ${border}`, whiteSpace:"nowrap", display:"inline-flex", alignItems:"center", gap:4 }}>
      <span>{emoji}</span><span>{label}</span>
    </span>
  );
}

// ─── Shared styles ──────────────────────────────────────────────────────────
const S = {
  page: { minHeight:"100vh", background:"#0B0D14", color:"#E8EBF4", fontFamily:"-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif" },
  surface: { background:"#13151F", border:"1px solid #252836", borderRadius:16 },
  input: { width:"100%", background:"#1C1E2A", border:"1px solid #252836", borderRadius:10, padding:"10px 14px", color:"#E8EBF4", fontSize:14, outline:"none", boxSizing:"border-box", transition:"border-color 0.15s" },
  select: { width:"100%", background:"#1C1E2A", border:"1px solid #252836", borderRadius:10, padding:"10px 14px", color:"#E8EBF4", fontSize:14, outline:"none", cursor:"pointer", boxSizing:"border-box" },
  label: { fontSize:11, fontWeight:700, color:"#5C6280", letterSpacing:"0.07em", textTransform:"uppercase", marginBottom:6, display:"block" },
  btnPrimary: { background:"linear-gradient(135deg,#6366F1,#4F46E5)", color:"#fff", border:"none", borderRadius:10, padding:"11px 20px", fontSize:14, fontWeight:700, cursor:"pointer" },
  btnGhost: { background:"#1C1E2A", color:"#9CA3AF", border:"1px solid #252836", borderRadius:10, padding:"11px 20px", fontSize:14, fontWeight:600, cursor:"pointer" },
};

// ─── SETUP SCREEN ──────────────────────────────────────────────────────────
function SetupScreen({ onConnect }) {
  const [url, setUrl] = useState("");
  const [code, setCode] = useState("");
  const [err, setErr] = useState("");
  const [busy, setBusy] = useState(false);
  const [showInstr, setShowInstr] = useState(false);

  const connect = async () => {
    if (!url.trim() || !code.trim()) { setErr("Both fields are required."); return; }
    setBusy(true); setErr("");
    try {
      const cleanUrl  = url.trim().replace(/\/$/, "");
      const cleanCode = code.trim().toLowerCase().replace(/[^a-z0-9_]/g, "") || "home";
      FB.url = cleanUrl; FB.code = cleanCode;
      await fGet("ping"); // test call — will return null if path doesn't exist, which is fine
      ls.set("tvtrack_cfg", { url: cleanUrl, code: cleanCode });
      onConnect();
    } catch (e) {
      // Firebase returns 401/403 if rules block; any other error = network/URL issue
      if (e?.status === 401 || e?.status === 403) {
        setErr("Permission denied — set database rules to allow read/write (test mode) in Firebase console.");
      } else {
        setErr("Could not connect. Double-check the database URL and try again.");
      }
    }
    setBusy(false);
  };

  return (
    <div style={{ ...S.page, display:"flex", alignItems:"center", justifyContent:"center", padding:20 }}>
      <div style={{ ...S.surface, maxWidth:460, width:"100%", padding:28 }}>
        <div style={{ textAlign:"center", marginBottom:28 }}>
          <div style={{ fontSize:52, marginBottom:10, lineHeight:1 }}>📺</div>
          <h1 style={{ margin:0, fontSize:28, fontWeight:900, background:"linear-gradient(135deg,#818CF8 30%,#60A5FA)", WebkitBackgroundClip:"text", WebkitTextFillColor:"transparent" }}>WatchList</h1>
          <p style={{ color:"#5C6280", marginTop:8, fontSize:14 }}>Shared TV tracking for your whole household</p>
        </div>

        <div style={{ marginBottom:16 }}>
          <label style={S.label}>Firebase Realtime Database URL</label>
          <input style={S.input} value={url} onChange={e => setUrl(e.target.value)}
            placeholder="https://your-project-default-rtdb.firebaseio.com"
            onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"} />
        </div>

        <div style={{ marginBottom:20 }}>
          <label style={S.label}>Household Code</label>
          <input style={S.input} value={code} onChange={e => setCode(e.target.value)}
            placeholder="e.g.  home  or  smithfamily"
            onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"}
            onKeyDown={e => e.key === "Enter" && connect()} />
          <p style={{ fontSize:12, color:"#5C6280", marginTop:6, lineHeight:1.5 }}>Letters and numbers only — all household members use the same code to see shared data.</p>
        </div>

        {err && (
          <div style={{ background:"rgba(239,68,68,0.1)", border:"1px solid rgba(239,68,68,0.3)", borderRadius:10, padding:"10px 14px", color:"#F87171", fontSize:13, marginBottom:16, lineHeight:1.5 }}>
            {err}
          </div>
        )}

        <button onClick={connect} disabled={busy} style={{ ...S.btnPrimary, width:"100%", opacity:busy?0.7:1, fontSize:15 }}>
          {busy ? "Connecting…" : "Connect & Continue →"}
        </button>

        <div style={{ marginTop:20, borderTop:"1px solid #252836", paddingTop:16 }}>
          <button onClick={() => setShowInstr(v => !v)} style={{ background:"none", border:"none", color:"#5C6280", fontSize:13, cursor:"pointer", padding:0, display:"flex", alignItems:"center", gap:6 }}>
            <span style={{ transition:"transform 0.2s", display:"inline-block", transform:showInstr?"rotate(90deg)":"rotate(0)" }}>▶</span>
            How to set up a free Firebase database
          </button>
          {showInstr && (
            <ol style={{ color:"#9CA3AF", fontSize:13, lineHeight:1.8, paddingLeft:20, marginTop:12 }}>
              <li>Go to <a href="https://console.firebase.google.com" target="_blank" rel="noopener" style={{ color:"#818CF8" }}>console.firebase.google.com</a> and sign in</li>
              <li>Click <strong style={{ color:"#E8EBF4" }}>Add project</strong> — name it anything (Analytics optional)</li>
              <li>In the left sidebar: <strong style={{ color:"#E8EBF4" }}>Build → Realtime Database</strong></li>
              <li>Click <strong style={{ color:"#E8EBF4" }}>Create Database</strong> → pick a region → <em>Start in test mode</em> → Enable</li>
              <li>Copy the URL shown (e.g. <code style={{ color:"#818CF8" }}>https://myproject-rtdb.firebaseio.com</code>)</li>
              <li>Paste it above — every household member uses the same URL and code</li>
            </ol>
          )}
        </div>
      </div>
    </div>
  );
}

// ─── MEMBERS SCREEN ─────────────────────────────────────────────────────────
function MembersScreen({ onSelect }) {
  const [members, setMembers] = useState({});
  const [newName, setNewName] = useState("");
  const [busy, setBusy] = useState(false);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fGet("members").then(d => { setMembers(d || {}); setLoading(false); }).catch(() => setLoading(false));
  }, []);

  const addMember = async () => {
    const name = newName.trim(); if (!name) return;
    setBusy(true);
    const color = MEMBER_COLORS[Object.keys(members).length % MEMBER_COLORS.length];
    const data  = { name, color, joinedAt: Date.now() };
    const r     = await fPost("members", data);
    const member = { id: r.name, ...data };
    ls.set("tvtrack_me", member);
    onSelect(member);
    setBusy(false);
  };

  const pick = id => {
    const member = { id, ...members[id] };
    ls.set("tvtrack_me", member);
    onSelect(member);
  };

  return (
    <div style={{ ...S.page, display:"flex", alignItems:"center", justifyContent:"center", padding:20 }}>
      <div style={{ ...S.surface, maxWidth:460, width:"100%", padding:28 }}>
        <div style={{ textAlign:"center", marginBottom:24 }}>
          <div style={{ fontSize:40, marginBottom:10 }}>👋</div>
          <h2 style={{ margin:0, fontSize:22, fontWeight:800 }}>Who's watching?</h2>
          <p style={{ color:"#5C6280", marginTop:6, fontSize:14 }}>Pick your profile or add yourself</p>
        </div>

        {loading ? (
          <div style={{ textAlign:"center", color:"#5C6280", padding:"20px 0" }}>Loading…</div>
        ) : (
          <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(110px,1fr))", gap:10, marginBottom:24 }}>
            {Object.entries(members).map(([id, m]) => (
              <button key={id} onClick={() => pick(id)} style={{ background:"#1C1E2A", border:"1px solid #252836", borderRadius:14, padding:"18px 8px", cursor:"pointer", display:"flex", flexDirection:"column", alignItems:"center", gap:9, transition:"all 0.15s", outline:"none" }}
                onMouseEnter={e => { e.currentTarget.style.borderColor = m.color; e.currentTarget.style.background = m.color+"18"; }}
                onMouseLeave={e => { e.currentTarget.style.borderColor = "#252836"; e.currentTarget.style.background = "#1C1E2A"; }}>
                <Avatar name={m.name} color={m.color} size={46} />
                <span style={{ color:"#E8EBF4", fontSize:13, fontWeight:700, textAlign:"center", wordBreak:"break-word" }}>{m.name}</span>
              </button>
            ))}
          </div>
        )}

        <div style={{ borderTop:"1px solid #252836", paddingTop:20 }}>
          <label style={S.label}>Add new member</label>
          <div style={{ display:"flex", gap:8 }}>
            <input style={{ ...S.input, flex:1 }} value={newName} onChange={e => setNewName(e.target.value)}
              placeholder="Your name"
              onKeyDown={e => e.key === "Enter" && addMember()}
              onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"} />
            <button onClick={addMember} disabled={busy||!newName.trim()} style={{ ...S.btnPrimary, whiteSpace:"nowrap", opacity:busy?0.7:1, flexShrink:0 }}>
              {busy ? "…" : "Join →"}
            </button>
          </div>
        </div>
      </div>
    </div>
  );
}

// ─── SHOW MODAL ─────────────────────────────────────────────────────────────
function ShowModal({ mode, initial, onSave, onClose }) {
  const [form, setForm] = useState(initial || { title:"", service:"netflix", status:"watching", season:1, episode:1, notes:"", type:"series" });
  const [busy, setBusy] = useState(false);
  const f = (k, v) => setForm(p => ({ ...p, [k]: v }));

  const save = async () => {
    if (!form.title.trim()) return;
    setBusy(true);
    await onSave({ ...form, title: form.title.trim(), season: +form.season || 1, episode: +form.episode || 1 });
    setBusy(false);
  };

  return (
    <div onClick={onClose} style={{ position:"fixed", inset:0, background:"rgba(0,0,0,0.75)", display:"flex", alignItems:"flex-end", justifyContent:"center", zIndex:400, backdropFilter:"blur(4px)" }}>
      <div onClick={e => e.stopPropagation()} style={{ ...S.surface, borderRadius:"18px 18px 0 0", width:"100%", maxWidth:520, maxHeight:"92vh", overflowY:"auto", padding:22 }}>
        {/* Header */}
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:22 }}>
          <h3 style={{ margin:0, fontSize:18, fontWeight:800 }}>{mode === "add" ? "＋ Add Show" : "✎ Edit Show"}</h3>
          <button onClick={onClose} style={{ background:"#1C1E2A", border:"1px solid #252836", borderRadius:8, padding:"5px 10px", color:"#9CA3AF", cursor:"pointer", fontSize:16 }}>✕</button>
        </div>

        <div style={{ display:"flex", flexDirection:"column", gap:16 }}>
          {/* Title */}
          <div>
            <label style={S.label}>Show Title</label>
            <input autoFocus style={S.input} value={form.title} onChange={e => f("title", e.target.value)}
              placeholder="e.g. The Bear, Severance, Shogun…"
              onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"} />
          </div>

          {/* Service */}
          <div>
            <label style={S.label}>Channel / Streaming Service</label>
            <select style={S.select} value={form.service} onChange={e => f("service", e.target.value)}>
              {SERVICES.map(s => <option key={s.id} value={s.id}>{s.name}</option>)}
            </select>
          </div>

          {/* Status */}
          <div>
            <label style={S.label}>Status</label>
            <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:8 }}>
              {Object.entries(STATUS).map(([k, st]) => (
                <button key={k} onClick={() => f("status", k)} style={{ padding:"10px 8px", borderRadius:10, cursor:"pointer", fontWeight:700, fontSize:13, transition:"all 0.15s", outline:"none",
                  border:`1px solid ${form.status===k ? st.border : "#252836"}`,
                  background: form.status===k ? st.bg : "#1C1E2A",
                  color: form.status===k ? st.color : "#5C6280" }}>
                  {st.emoji} {st.label}
                </button>
              ))}
            </div>
          </div>

          {/* Season / Episode */}
          <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:12 }}>
            <div>
              <label style={S.label}>Season</label>
              <input type="number" min={1} style={S.input} value={form.season} onChange={e => f("season", e.target.value)}
                onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"} />
            </div>
            <div>
              <label style={S.label}>Episode</label>
              <input type="number" min={1} style={S.input} value={form.episode} onChange={e => f("episode", e.target.value)}
                onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"} />
            </div>
          </div>

          {/* Notes */}
          <div>
            <label style={S.label}>Notes <span style={{ color:"#3A3D55", fontWeight:400 }}>(optional)</span></label>
            <textarea style={{ ...S.input, minHeight:68, resize:"vertical", lineHeight:1.5 }} value={form.notes}
              onChange={e => f("notes", e.target.value)} placeholder="Where you're up to, reminders, spoiler-free thoughts…"
              onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"} />
          </div>

          <button onClick={save} disabled={busy || !form.title.trim()} style={{ ...S.btnPrimary, width:"100%", fontSize:15, padding:"13px 20px", opacity:busy||!form.title.trim()?0.6:1 }}>
            {busy ? "Saving…" : mode === "add" ? "Add to List" : "Save Changes"}
          </button>
        </div>
      </div>
    </div>
  );
}

// ─── SHOW CARD ───────────────────────────────────────────────────────────────
function ShowCard({ id, show, members, onEdit, onDelete, onPatch }) {
  const svc   = SVC[show.service] || SVC.other;
  const st    = STATUS[show.status]  || STATUS.watching;
  const adder = members[show.addedBy];

  const btnStyle = (accent) => ({
    background: `rgba(${accent},0.12)`, border:`1px solid rgba(${accent},0.3)`,
    borderRadius:7, padding:"3px 9px", fontSize:12, fontWeight:700,
    color:`rgb(${accent})`, cursor:"pointer", transition:"all 0.15s", outline:"none",
  });
  const iconBtn = { background:"#1C1E2A", border:"1px solid #252836", borderRadius:7, padding:"5px 9px", fontSize:13, color:"#5C6280", cursor:"pointer", transition:"all 0.15s" };

  const showProgress = show.status !== "want";

  return (
    <div style={{ background:"#13151F", border:"1px solid #252836", borderRadius:14, overflow:"hidden", display:"flex", flexDirection:"column", transition:"border-color 0.2s" }}
      onMouseEnter={e => e.currentTarget.style.borderColor = svc.color+"60"}
      onMouseLeave={e => e.currentTarget.style.borderColor = "#252836"}>
      {/* Service colour bar */}
      <div style={{ height:4, background:svc.color }} />

      <div style={{ padding:"14px 14px 12px", flex:1, display:"flex", flexDirection:"column", gap:10 }}>
        {/* Top badges */}
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", gap:6, flexWrap:"wrap" }}>
          <span style={{ fontSize:11, fontWeight:700, padding:"3px 8px", borderRadius:100, background:svc.color+"22", color:svc.color, border:`1px solid ${svc.color}40`, whiteSpace:"nowrap" }}>
            {svc.name}
          </span>
          <Badge {...st} />
        </div>

        {/* Title */}
        <div style={{ fontSize:17, fontWeight:800, lineHeight:1.3, color:"#F0F3FF" }}>{show.title}</div>

        {/* Progress */}
        {showProgress && (
          <div style={{ display:"flex", alignItems:"center", gap:7, flexWrap:"wrap" }}>
            <span style={{ fontFamily:"'SF Mono',Menlo,monospace", fontSize:14, fontWeight:700, color:"#818CF8", background:"rgba(129,140,248,0.12)", padding:"4px 10px", borderRadius:8, letterSpacing:"0.03em" }}>
              S{String(show.season||1).padStart(2,"0")} · E{String(show.episode||1).padStart(2,"0")}
            </span>
            {show.status !== "finished" && <>
              <button onClick={() => onPatch(id, { episode:(show.episode||0)+1 })} style={btnStyle("129,140,248")}>＋Ep</button>
              <button onClick={() => onPatch(id, { season:(show.season||1)+1, episode:1 })} style={btnStyle("96,165,250")}>＋Season</button>
            </>}
          </div>
        )}

        {/* Status-specific quick actions */}
        {show.status === "want" && (
          <button onClick={() => onPatch(id, { status:"watching" })}
            style={{ alignSelf:"flex-start", background:"rgba(34,197,94,0.12)", border:"1px solid rgba(34,197,94,0.3)", borderRadius:8, padding:"5px 12px", fontSize:12, fontWeight:700, color:"#22C55E", cursor:"pointer" }}>
            ▶ Start Watching
          </button>
        )}
        {show.status === "paused" && (
          <button onClick={() => onPatch(id, { status:"watching" })}
            style={{ alignSelf:"flex-start", background:"rgba(34,197,94,0.12)", border:"1px solid rgba(34,197,94,0.3)", borderRadius:8, padding:"5px 12px", fontSize:12, fontWeight:700, color:"#22C55E", cursor:"pointer" }}>
            ▶ Resume
          </button>
        )}

        {/* Notes */}
        {show.notes ? (
          <div style={{ fontSize:12, color:"#5C6280", fontStyle:"italic", borderTop:"1px solid #1E2030", paddingTop:8, lineHeight:1.5 }}>{show.notes}</div>
        ) : null}

        {/* Footer */}
        <div style={{ display:"flex", alignItems:"center", justifyContent:"space-between", marginTop:"auto", paddingTop:6 }}>
          <div style={{ display:"flex", alignItems:"center", gap:6 }}>
            {adder && <Avatar name={adder.name} color={adder.color} size={20} />}
            <span style={{ fontSize:11, color:"#5C6280" }}>{adder?.name || "—"}</span>
          </div>
          <div style={{ display:"flex", gap:6 }}>
            {show.status === "watching" && (
              <button onClick={() => onPatch(id, { status:"paused" })} title="Pause" style={iconBtn}>⏸</button>
            )}
            {show.status !== "finished" && (
              <button onClick={() => onPatch(id, { status:"finished" })} title="Mark finished" style={{ ...iconBtn, color:"#A78BFA" }}>✓</button>
            )}
            {show.status === "finished" && (
              <button onClick={() => onPatch(id, { status:"watching", episode:1, season:1 })} title="Rewatch" style={{ ...iconBtn, color:"#22C55E" }}>↩</button>
            )}
            <button onClick={() => onEdit(id, show)} title="Edit" style={iconBtn}>✎</button>
            <button onClick={() => onDelete(id)} title="Remove" style={{ ...iconBtn, color:"#EF4444" }}>✕</button>
          </div>
        </div>
      </div>
    </div>
  );
}

// ─── MAIN SCREEN ─────────────────────────────────────────────────────────────
function MainScreen({ me, onSwitchMember }) {
  const [members, setMembers] = useState({});
  const [shows,   setShows]   = useState({});
  const [lastSync, setLastSync] = useState(null);
  const [syncing,  setSyncing]  = useState(false);

  const [statusFilter,  setStatusFilter]  = useState("all");
  const [svcFilter,     setSvcFilter]     = useState("all");
  const [memberFilter,  setMemberFilter]  = useState("all");
  const [search,        setSearch]        = useState("");

  const [modal,   setModal]   = useState(null); // null | "add" | "edit"
  const [editId,  setEditId]  = useState(null);
  const [editInit, setEditInit] = useState(null);

  const pollRef = useRef(null);

  const loadAll = useCallback(async () => {
    setSyncing(true);
    try {
      const [m, s] = await Promise.all([fGet("members"), fGet("shows")]);
      setMembers(m || {});
      setShows(s || {});
      setLastSync(new Date());
    } catch {}
    setSyncing(false);
  }, []);

  useEffect(() => {
    loadAll();
    pollRef.current = setInterval(loadAll, 30000);
    const onFocus = () => loadAll();
    window.addEventListener("focus", onFocus);
    return () => { clearInterval(pollRef.current); window.removeEventListener("focus", onFocus); };
  }, [loadAll]);

  // ── CRUD ──
  const saveShow = async (formData) => {
    if (!editId) {
      const data = { ...formData, addedBy: me.id, addedAt: Date.now(), updatedAt: Date.now() };
      const r = await fPost("shows", data);
      setShows(s => ({ ...s, [r.name]: data }));
    } else {
      const data = { ...formData, updatedAt: Date.now() };
      await fPatch(`shows/${editId}`, data);
      setShows(s => ({ ...s, [editId]: { ...s[editId], ...data } }));
    }
    setModal(null);
  };

  const deleteShow = async id => {
    if (!window.confirm("Remove this show from the shared list?")) return;
    await fDel(`shows/${id}`);
    setShows(s => { const n = { ...s }; delete n[id]; return n; });
  };

  const patchShow = async (id, upd) => {
    const data = { ...upd, updatedAt: Date.now() };
    await fPatch(`shows/${id}`, data);
    setShows(s => ({ ...s, [id]: { ...s[id], ...data } }));
  };

  const openEdit = (id, show) => {
    setEditId(id);
    setEditInit({ title:show.title, service:show.service, status:show.status, season:show.season||1, episode:show.episode||1, notes:show.notes||"" });
    setModal("edit");
  };

  // ── Filtering ──
  const q = search.trim().toLowerCase();
  const visible = Object.entries(shows)
    .filter(([, s]) => {
      if (statusFilter !== "all" && s.status !== statusFilter) return false;
      if (svcFilter    !== "all" && s.service !== svcFilter)   return false;
      if (memberFilter !== "all" && s.addedBy !== memberFilter) return false;
      if (q && !s.title.toLowerCase().includes(q))              return false;
      return true;
    })
    .sort(([,a],[,b]) => (b.updatedAt||0) - (a.updatedAt||0));

  const counts = Object.values(shows).reduce((acc, s) => { acc[s.status] = (acc[s.status]||0)+1; return acc; }, {});
  const totalShows = Object.keys(shows).length;
  const usedSvcs = [...new Set(Object.values(shows).map(s => s.service))].filter(Boolean);

  return (
    <div style={S.page}>
      {/* ── Header ── */}
      <div style={{ background:"#0B0D14", borderBottom:"1px solid #1C1E2A", position:"sticky", top:0, zIndex:200 }}>
        <div style={{ maxWidth:960, margin:"0 auto", padding:"11px 14px", display:"flex", alignItems:"center", gap:10 }}>
          <span style={{ fontSize:19, fontWeight:900, background:"linear-gradient(135deg,#818CF8 20%,#60A5FA)", WebkitBackgroundClip:"text", WebkitTextFillColor:"transparent", flex:1, letterSpacing:"-0.5px" }}>
            📺 WatchList
          </span>
          <span style={{ fontSize:11, color: syncing ? "#818CF8" : "#3A3D55" }}>
            {syncing ? "Syncing…" : lastSync ? `↻ ${lastSync.toLocaleTimeString([],{hour:"2-digit",minute:"2-digit"})}` : ""}
          </span>
          <button onClick={loadAll} title="Refresh now" style={{ background:"none", border:"none", color:"#5C6280", cursor:"pointer", fontSize:17, padding:"2px 4px", lineHeight:1 }}>↻</button>
          <button onClick={onSwitchMember} style={{ background:"#1C1E2A", border:"1px solid #252836", borderRadius:24, padding:"5px 11px 5px 6px", cursor:"pointer", display:"flex", alignItems:"center", gap:7 }}>
            <Avatar name={me.name} color={me.color} size={24} />
            <span style={{ color:"#E8EBF4", fontSize:13, fontWeight:700 }}>{me.name}</span>
          </button>
        </div>
      </div>

      <div style={{ maxWidth:960, margin:"0 auto", padding:"14px 12px 100px" }}>
        {/* ── Status stat tiles (also act as filters) ── */}
        <div style={{ display:"grid", gridTemplateColumns:"repeat(4,1fr)", gap:8, marginBottom:14 }}>
          {Object.entries(STATUS).map(([k, st]) => {
            const active = statusFilter === k;
            return (
              <button key={k} onClick={() => setStatusFilter(active ? "all" : k)} style={{ background: active ? st.bg : "#13151F", border:`1px solid ${active ? st.border : "#252836"}`, borderRadius:13, padding:"10px 6px", cursor:"pointer", textAlign:"center", transition:"all 0.18s", outline:"none" }}>
                <div style={{ fontSize:20, fontWeight:900, color:st.color, lineHeight:1 }}>{counts[k]||0}</div>
                <div style={{ fontSize:10, color: active ? st.color : "#5C6280", fontWeight:700, marginTop:3, lineHeight:1.2 }}>{st.label}</div>
              </button>
            );
          })}
        </div>

        {/* ── Filter bar ── */}
        <div style={{ display:"flex", gap:8, marginBottom:10, flexWrap:"wrap" }}>
          <input style={{ ...S.input, flex:2, minWidth:140 }} value={search} onChange={e => setSearch(e.target.value)}
            placeholder="🔍  Search shows…"
            onFocus={e => e.target.style.borderColor="#6366F1"} onBlur={e => e.target.style.borderColor="#252836"} />
          <select style={{ ...S.select, flex:1, minWidth:130 }} value={svcFilter} onChange={e => setSvcFilter(e.target.value)}>
            <option value="all">All Services</option>
            {usedSvcs.map(id => <option key={id} value={id}>{SVC[id]?.name||id}</option>)}
          </select>
          <select style={{ ...S.select, flex:1, minWidth:120 }} value={memberFilter} onChange={e => setMemberFilter(e.target.value)}>
            <option value="all">All Members</option>
            {Object.entries(members).map(([id,m]) => <option key={id} value={id}>{m.name}</option>)}
          </select>
          {(statusFilter!=="all"||svcFilter!=="all"||memberFilter!=="all"||search) && (
            <button onClick={() => { setStatusFilter("all"); setSvcFilter("all"); setMemberFilter("all"); setSearch(""); }}
              style={{ ...S.btnGhost, whiteSpace:"nowrap", fontSize:13, padding:"8px 12px" }}>✕ Clear</button>
          )}
        </div>

        {/* ── Show count line ── */}
        <div style={{ fontSize:13, color:"#3A3D55", marginBottom:12 }}>
          {visible.length} of {totalShows} show{totalShows!==1?"s":""}
          {statusFilter!=="all" && <span style={{ color: STATUS[statusFilter].color }}> · {STATUS[statusFilter].label}</span>}
        </div>

        {/* ── Empty state ── */}
        {visible.length === 0 && (
          <div style={{ textAlign:"center", padding:"70px 20px", color:"#5C6280" }}>
            <div style={{ fontSize:56, marginBottom:12 }}>🎬</div>
            {totalShows === 0
              ? <><div style={{ fontSize:16, fontWeight:700, color:"#9CA3AF" }}>No shows added yet</div><div style={{ fontSize:13, marginTop:6 }}>Tap + to add your first show to the household list</div></>
              : <><div style={{ fontSize:16, fontWeight:700, color:"#9CA3AF" }}>No matches</div><div style={{ fontSize:13, marginTop:6 }}>Try adjusting your filters</div></>
            }
          </div>
        )}

        {/* ── Show grid ── */}
        <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(270px,1fr))", gap:12 }}>
          {visible.map(([id, show]) => (
            <ShowCard key={id} id={id} show={show} members={members}
              onEdit={openEdit} onDelete={deleteShow} onPatch={patchShow} />
          ))}
        </div>
      </div>

      {/* ── FAB ── */}
      <button onClick={() => { setEditId(null); setEditInit(null); setModal("add"); }}
        style={{ position:"fixed", bottom:24, right:24, width:58, height:58, borderRadius:"50%", background:"linear-gradient(135deg,#818CF8,#4F46E5)", border:"none", color:"#fff", fontSize:28, fontWeight:300, cursor:"pointer", boxShadow:"0 4px 24px rgba(99,102,241,0.55)", display:"flex", alignItems:"center", justifyContent:"center", zIndex:300, lineHeight:1 }}>
        +
      </button>

      {/* ── Add / Edit Modal ── */}
      {modal && (
        <ShowModal mode={modal} initial={editInit} onSave={saveShow} onClose={() => setModal(null)} />
      )}
    </div>
  );
}

// ─── ROOT APP ────────────────────────────────────────────────────────────────
export default function App() {
  const [screen, setScreen] = useState("init"); // init | setup | members | main
  const [me, setMe] = useState(null);

  useEffect(() => {
    const cfg = ls.get("tvtrack_cfg");
    const savedMe = ls.get("tvtrack_me");
    if (cfg?.url && cfg?.code) {
      FB.url = cfg.url; FB.code = cfg.code;
      if (savedMe?.id) { setMe(savedMe); setScreen("main"); }
      else setScreen("members");
    } else {
      setScreen("setup");
    }
  }, []);

  const handleConnect = () => setScreen("members");

  const handleSelectMember = (member) => {
    setMe(member);
    setScreen("main");
  };

  const handleSwitchMember = () => {
    ls.del("tvtrack_me");
    setMe(null);
    setScreen("members");
  };

  if (screen === "init") return (
    <div style={{ ...S.page, display:"flex", alignItems:"center", justifyContent:"center", color:"#5C6280" }}>Loading…</div>
  );
  if (screen === "setup")   return <SetupScreen onConnect={handleConnect} />;
  if (screen === "members") return <MembersScreen onSelect={handleSelectMember} />;
  if (screen === "main" && me) return <MainScreen me={me} onSwitchMember={handleSwitchMember} />;
  return null;
}
