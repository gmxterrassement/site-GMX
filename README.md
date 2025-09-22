# site-GMXimport React, { useEffect, useMemo, useRef, useState } from "react";
import { motion } from "framer-motion";
import { Menu, X, Wrench, Tractor, Building2, Hammer, Phone, Mail, MapPin, Download, Upload, ChevronRight, CheckCircle2, Edit3, Image as ImageIcon, Settings2, LogOut } from "lucide-react";

// =====================================================================================
//  LES ENTREPRISES GMX — SITE VITRINE ÉDITABLE (SPA)
//  -------------------------------------------------
//  ✅ 100% en une page, responsive, Tailwind CSS
//  ✅ Mode Admin intégré: modifiez textes/images, enregistrez (localStorage), import/export JSON
//  ✅ Pas de backend requis (formulaire via mailto: ou copier-coller)
//  ✅ SEO de base (balises meta dans <Head> virtuel) + structure claire
//
//  📌 COMMENT UTILISER
//  1) Cliquez sur "Admin" en haut à droite → Entrez le mot de passe par défaut: gmx
//  2) Activez le mode édition (toggle). Cliquez sur n'importe quel texte pour l'éditer (inline).
//  3) Ajoutez/retirez des Services et des Projets depuis l'onglet Contenu.
//  4) Bouton "Sauvegarder" → stocke le contenu dans votre navigateur (localStorage).
//  5) Export/Import JSON (onglet Sauvegardes) pour transférer le contenu entre ordinateurs.
//  6) Changez le mot de passe Admin dans Réglages → retapez pour confirmer.
//
//  💡 DÉPLOIEMENT
//  - Exportez ce composant dans un projet React/Vite/Next et déployez sur Netlify/Vercel/GitHub Pages.
//  - Le site reste éditable côté client. Pour un CMS (Netlify CMS/Contentful), on pourra migrer facilement.
//
//  ✉️ Formulaire
//  - Par défaut, le bouton "Envoyer" ouvre votre client courriel (mailto:). Vous pouvez remplacer par un service
//    d'email sans backend (Formspree, Getform) plus tard.
// =====================================================================================

// Utilitaires simples
const cx = (...cls) => cls.filter(Boolean).join(" ");
const uid = () => Math.random().toString(36).slice(2, 9);

const DEFAULT_DATA = {
  brand: {
    name: "Les Entreprises GMX",
    tagline: "Excavation • Construction • Aménagement paysager",
    logoUrl: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABAAAAAQACAIAAADwf7...AAAR2p1bWRjMm1hABEAEIAAAKoAOJtxA3VybjpjMnBhOjZkZTM4MGExLWM3Mm...",
    primaryColor: "#4b5563", // cyan-500
  },
  contact: {
    phone: "+1 (514) 555-0185",
    email: "info@gmx-entreprises.com",
    address: "Rigaud, Québec",
    hours: "Lun–Ven 7h–17h",
    mapLink: "https://maps.google.com/?q=Rigaud+QC",
  },
  hero: {
    title: "Nous bâtissons solide et propre.",
    subtitle:
      "Excavation, terrassement, drains français, murets, pavé uni, dalles, mini-excavation et projets clé en main.",
    ctaLabel: "Obtenir une soumission",
    ctaAnchor: "#contact",
    backgroundUrl:
      "https://images.unsplash.com/photo-1591453089816-0fbb971b851f?q=80&w=1600&auto=format&fit=crop",
  },
  highlights: [
    { id: uid(), icon: "Tractor", text: "Excavation précise et sécuritaire" },
    { id: uid(), icon: "Hammer", text: "Finitions durables, garanties" },
    { id: uid(), icon: "Building2", text: "Gestion complète du chantier" },
  ],
  services: [
    {
      id: uid(),
      title: "Excavation & Drain français",
      desc:
        "Remplacement de drains, tranchées, imperméabilisation, remblai contrôlé, stabilisation de dalle.",
      icon: "Tractor",
      bullets: [
        "Inspection accès au drain (ocre ferreuse, obstructions)",
        "Hydro-excavation et protection des ouvrages",
        "Membranes, isolants et reprise de pente",
      ],
    },
    {
      id: uid(),
      title: "Terrassement & Pavé uni",
      desc:
        "Conception et pose de pavé, murets, marches, dalles structurales, nivellement, gestion des eaux.",
      icon: "Wrench",
      bullets: [
        "Pavé 60/80 mm, bordures, compactage professionnel",
        "Murets autoportants, escaliers et paliers",
        "Gazons, plantations et éclairage intégré",
      ],
    },
    {
      id: uid(),
      title: "Construction légère",
      desc:
        "Dalles de béton, petites structures, abris, clôtures et travaux connexes aux aménagements.",
      icon: "Building2",
      bullets: [
        "Planage, coffrage et armature",
        "Ancrages, pieux vissés (coordination)",
        "Finitions propres et garanties",
      ],
    },
  ],
  projects: [
    {
      id: uid(),
      title: "Remplacement drain français & stabilisation de dalle",
      location: "Rigaud",
      image:
        "https://images.unsplash.com/photo-1605810230434-7631ac76ec82?q=80&w=1200&auto=format&fit=crop",
      tags: ["Excavation", "Drain", "Structure"],
    },
    {
      id: uid(),
      title: "Cour avant complète: pavé, muret et éclairage",
      location: "Vaudreuil-Dorion",
      image:
        "https://images.unsplash.com/photo-1616486701797-0f33f4f2d660?q=80&w=1200&auto=format&fit=crop",
      tags: ["Pavé uni", "Muret", "Éclairage"],
    },
    {
      id: uid(),
      title: "Patio béton et marches sur pieux",
      location: "Saint-Clet",
      image:
        "https://images.unsplash.com/photo-1621905251918-48416bd8575a?q=80&w=1200&auto=format&fit=crop",
      tags: ["Béton", "Pieux", "Aménagement"],
    },
  ],
  about: {
    title: "Une équipe rigoureuse, un chantier impeccable",
    body:
      "Entreprise locale, assurée et rigoureuse, Les Entreprises GMX livrent des chantiers propres, sécuritaires et durables. Notre approche: bien planifier, bien exécuter, bien nettoyer. Nous travaillons avec des partenaires certifiés (pieux, béton, électricité) et nous respectons vos délais.",
    badges: ["RBQ en règle", "Assurance responsabilité", "Soumission détaillée"],
  },
  faq: [
    {
      id: uid(),
      q: "Comment obtenir une soumission?",
      a: "Cliquez sur le bouton 'Obtenir une soumission', décrivez votre projet et joignez quelques photos. Nous vous rappelons rapidement.",
    },
    { id: uid(), q: "Couvrez-vous ma région?", a: "Rigaud, Vaudreuil-Soulanges, Salaberry-de-Valleyfield et environs." },
    { id: uid(), q: "Offrez-vous des garanties?", a: "Oui, selon les travaux: base granulaire, pose, et certaines fournitures." },
  ],
  footer: {
    legal: "© " + new Date().getFullYear() + " Les Entreprises GMX. Tous droits réservés.",
    links: [
      { id: uid(), label: "Politique de confidentialité", href: "#" },
      { id: uid(), label: "Conditions", href: "#" },
    ],
  },
  admin: {
    password: "gmx",
  },
};

// LocalStorage keys
const LS_KEY = "gmx_site_data_v1";

// Icônes dynamiques
const ICONS = { Menu, X, Wrench, Tractor, Building2, Hammer, Phone, Mail, MapPin, Download, Upload, ChevronRight, CheckCircle2, Edit3, ImageIcon, Settings2, LogOut };
const Icon = ({ name, className }) => {
  const C = ICONS[name] || ICONS.Hammer;
  return <C className={className} />;
};

// Composant contenteditable simple
function Editable({ html, onChange, enabled, className, placeholder }) {
  const ref = useRef(null);
  useEffect(() => {
    if (ref.current && html !== ref.current.innerHTML) {
      ref.current.innerHTML = html || "";
    }
  }, [html]);
  return (
    <div
      ref={ref}
      contentEditable={enabled}
      suppressContentEditableWarning
      onInput={(e) => onChange?.(e.currentTarget.innerHTML)}
      className={cx(
        className,
        enabled ? "outline outline-1 outline-dashed outline-sky-400/60 rounded-md focus:outline-sky-500" : ""
      )}
      data-placeholder={placeholder}
    />
  );
}

export default function GMXSite() {
  const [data, setData] = useState(DEFAULT_DATA);
  const [navOpen, setNavOpen] = useState(false);
  const [adminAsk, setAdminAsk] = useState(false);
  const [adminMode, setAdminMode] = useState(false);
  const [editEnabled, setEditEnabled] = useState(false);
  const [activeTab, setActiveTab] = useState("contenu");
  const [pwdInput, setPwdInput] = useState("");
  const [jsonText, setJsonText] = useState("");

  // Charger depuis localStorage
  useEffect(() => {
    try {
      const raw = localStorage.getItem(LS_KEY);
      if (raw) setData(JSON.parse(raw));
    } catch (_) {}
  }, []);

  // Couleur primaire
  const cssVars = useMemo(() => ({
    "--brand": data.brand?.primaryColor || "#0ea5e9",
  }), [data.brand?.primaryColor]);

  const saveLS = () => {
    localStorage.setItem(LS_KEY, JSON.stringify(data));
    alert("Contenu sauvegardé dans ce navigateur.");
  };

  const exportJSON = () => {
    setJsonText(JSON.stringify(data, null, 2));
  };

  const importJSON = () => {
    try {
      const obj = JSON.parse(jsonText);
      setData(obj);
      localStorage.setItem(LS_KEY, JSON.stringify(obj));
      alert("Import terminé.");
    } catch (e) {
      alert("JSON invalide");
    }
  };

  const clearLS = () => {
    if (confirm("Effacer le contenu sauvegardé de ce navigateur?")) {
      localStorage.removeItem(LS_KEY);
      alert("OK. Rechargez la page pour revenir aux valeurs par défaut.");
    }
  };

  // Modificateurs génériques
  const set = (path, value) => {
    setData((prev) => {
      const copy = structuredClone(prev);
      const keys = path.split(".");
      let obj = copy;
      for (let i = 0; i < keys.length - 1; i++) obj = obj[keys[i]];
      obj[keys.at(-1)] = value;
      return copy;
    });
  };

  const addService = () => {
    setData((prev) => ({
      ...prev,
      services: [
        ...prev.services,
        { id: uid(), title: "Nouveau service", desc: "Description...", icon: "Wrench", bullets: ["Point 1", "Point 2"] },
      ],
    }));
  };

  const rmService = (id) => setData((p) => ({ ...p, services: p.services.filter((s) => s.id !== id) }));

  const addProject = () => {
    setData((prev) => ({
      ...prev,
      projects: [
        ...prev.projects,
        { id: uid(), title: "Nouveau projet", location: "Ville", image: "", tags: ["Tag"] },
      ],
    }));
  };

  const rmProject = (id) => setData((p) => ({ ...p, projects: p.projects.filter((s) => s.id !== id) }));

  // Auth admin très simple
  const tryLogin = () => {
    if (pwdInput === (data.admin?.password || "gmx")) {
      setAdminMode(true);
      setAdminAsk(false);
      setPwdInput("");
    } else alert("Mot de passe invalide");
  };

  const logout = () => {
    setAdminMode(false);
    setEditEnabled(false);
  };

  // Couleur dynamique
  const brandStyle = { color: "var(--brand)" };

  return (
    <div style={cssVars} className="min-h-screen bg-white text-slate-800">
      {/* HEAD rudimentaire */}
      <title>{data.brand.name} — Excavation & Aménagement</title>
      <meta name="description" content={data.hero.subtitle} />

      {/* Barre supérieure */}
      <header className="sticky top-0 z-50 bg-white/90 backdrop-blur border-b">
        <div className="max-w-7xl mx-auto px-4 py-3 flex items-center gap-4">
          <button className="md:hidden p-2 rounded-lg hover:bg-slate-100" onClick={() => setNavOpen((v) => !v)}>
            <Menu className="w-5 h-5" />
          </button>
          <div className="flex items-center gap-3">
            <img src={data.brand.logoUrl} alt="logo" className="w-10 h-10 rounded-xl object-cover shadow" />
            <div>
              <div className="font-semibold leading-tight" dangerouslySetInnerHTML={{ __html: data.brand.name }} />
              <div className="text-xs text-slate-500" dangerouslySetInnerHTML={{ __html: data.brand.tagline }} />
            </div>
          </div>
          <nav className={cx("ml-auto md:flex items-center gap-6 text-sm", navOpen ? "block" : "hidden md:flex")}>            
            <a href="#services" className="hover:underline">Services</a>
            <a href="#projets" className="hover:underline">Réalisations</a>
            <a href="#about" className="hover:underline">À propos</a>
            <a href="#faq" className="hover:underline">FAQ</a>
            <a href="#contact" className="btn-primary">Contact</a>
            <span className="hidden md:inline text-slate-300">|</span>
            <button onClick={() => (adminMode ? setEditEnabled((v) => !v) : setAdminAsk(true))} className="inline-flex items-center gap-2 text-xs px-3 py-1.5 rounded-full border hover:bg-slate-50">
              <Settings2 className="w-4 h-4" /> {adminMode ? (editEnabled ? "Édition: ON" : "Édition: OFF") : "Admin"}
            </button>
            {adminMode && (
              <button onClick={logout} className="inline-flex items-center gap-2 text-xs px-3 py-1.5 rounded-full border hover:bg-slate-50">
                <LogOut className="w-4 h-4" /> Quitter
              </button>
            )}
          </nav>
        </div>
      </header>

      {/* HERO */}
      <section id="hero" className="relative">
        <img src={data.hero.backgroundUrl} alt="chantier" className="absolute inset-0 w-full h-full object-cover -z-10" />
        <div className="absolute inset-0 bg-black/40 -z-10" />
        <div className="max-w-7xl mx-auto px-4 py-24 md:py-36">
          <motion.h1 initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.6 }} className="text-3xl md:text-5xl font-bold text-white max-w-3xl">
            <Editable enabled={editEnabled} html={data.hero.title} onChange={(v) => set("hero.title", v)} />
          </motion.h1>
          <p className="text-white/90 mt-4 max-w-2xl">
            <Editable enabled={editEnabled} html={data.hero.subtitle} onChange={(v) => set("hero.subtitle", v)} />
          </p>
          <div className="mt-8 flex flex-wrap items-center gap-3">
            <a href={data.hero.ctaAnchor} className="inline-flex items-center gap-2 px-5 py-3 rounded-2xl bg-[var(--brand)] text-white font-medium shadow hover:opacity-95">
              <ChevronRight className="w-5 h-5" />
              <Editable enabled={editEnabled} html={data.hero.ctaLabel} onChange={(v) => set("hero.ctaLabel", v)} />
            </a>
            {editEnabled && (
              <div className="flex items-center gap-2 text-white/90">
                <ImageIcon className="w-5 h-5" />
                <input className="text-black rounded px-2 py-1" value={data.hero.backgroundUrl} onChange={(e) => set("hero.backgroundUrl", e.target.value)} />
              </div>
            )}
          </div>

          <div className="mt-10 grid grid-cols-1 md:grid-cols-3 gap-4">
            {data.highlights.map((h) => (
              <div key={h.id} className="flex items-center gap-3 bg-white/10 backdrop-blur rounded-2xl p-4 border border-white/10">
                <Icon name={h.icon} className="w-6 h-6 text-white" />
                <div className="text-white/90">
                  <Editable enabled={editEnabled} html={h.text} onChange={(v) => setData((p) => ({ ...p, highlights: p.highlights.map((x) => (x.id === h.id ? { ...x, text: v } : x)) }))} />
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* SERVICES */}
      <section id="services" className="py-16 md:py-20 bg-slate-50">
        <div className="max-w-7xl mx-auto px-4">
          <h2 className="text-2xl md:text-3xl font-bold">Services</h2>
          <p className="text-slate-600 mt-2">De la prise de mesure à la livraison, nous gérons proprement chaque étape.</p>

          <div className="grid md:grid-cols-3 gap-6 mt-8">
            {data.services.map((s) => (
              <div key={s.id} className="rounded-2xl bg-white border p-6 shadow-sm">
                <div className="flex items-center gap-3">
                  <Icon name={s.icon} className="w-6 h-6" />
                  <h3 className="font-semibold text-lg"><Editable enabled={editEnabled} html={s.title} onChange={(v) => setData((p) => ({ ...p, services: p.services.map((x) => (x.id === s.id ? { ...x, title: v } : x)) }))} /></h3>
                </div>
                <p className="text-slate-600 mt-2"><Editable enabled={editEnabled} html={s.desc} onChange={(v) => setData((p) => ({ ...p, services: p.services.map((x) => (x.id === s.id ? { ...x, desc: v } : x)) }))} /></p>
                <ul className="mt-4 space-y-2">
                  {s.bullets.map((b, i) => (
                    <li key={i} className="flex items-start gap-2"><CheckCircle2 className="w-4 h-4 mt-1" /><Editable enabled={editEnabled} html={b} onChange={(v) => setData((p) => ({ ...p, services: p.services.map((x) => (x.id === s.id ? { ...x, bullets: x.bullets.map((bb,ii)=> ii===i? v: bb) } : x)) }))} /></li>
                  ))}
                </ul>
                {editEnabled && (
                  <div className="mt-4 flex items-center gap-2">
                    <button className="px-3 py-1.5 text-xs rounded-full border" onClick={() => setData((p)=> ({...p, services: p.services.map(x=> x.id===s.id? {...x, bullets:[...x.bullets, "Nouveau point"]}: x)}))}>+ point</button>
                    <button className="px-3 py-1.5 text-xs rounded-full border" onClick={() => rmService(s.id)}>Supprimer</button>
                    <select className="px-2 py-1 text-xs rounded border" value={s.icon} onChange={(e)=> setData((p)=> ({...p, services: p.services.map(x=> x.id===s.id? {...x, icon: e.target.value}: x)}))}>
                      {Object.keys(ICONS).map(k=> <option key={k} value={k}>{k}</option>)}
                    </select>
                  </div>
                )}
              </div>
            ))}
          </div>
          {editEnabled && (
            <div className="mt-6">
              <button className="px-4 py-2 rounded-xl border" onClick={addService}>+ Ajouter un service</button>
            </div>
          )}
        </div>
      </section>

      {/* PROJETS */}
      <section id="projets" className="py-16 md:py-20">
        <div className="max-w-7xl mx-auto px-4">
          <h2 className="text-2xl md:text-3xl font-bold">Réalisations récentes</h2>
          <p className="text-slate-600 mt-2">Un aperçu de chantiers livrés proprement et en sécurité.</p>

          <div className="grid md:grid-cols-3 gap-6 mt-8">
            {data.projects.map((p) => (
              <div key={p.id} className="group rounded-2xl overflow-hidden border bg-white shadow-sm">
                <div className="relative">
                  <img src={p.image || "https://images.unsplash.com/photo-1523419409543-9f3f0c4a7d99?q=80&w=1200&auto=format&fit=crop"} alt={p.title} className="w-full h-52 object-cover" />
                  {editEnabled && (
                    <div className="absolute top-2 right-2 bg-white/90 rounded-lg p-2 shadow">
                      <input className="text-xs px-2 py-1 border rounded" placeholder="URL image" value={p.image} onChange={(e)=> setData((prev)=> ({...prev, projects: prev.projects.map(x=> x.id===p.id? {...x, image: e.target.value}: x)}))} />
                    </div>
                  )}
                </div>
                <div className="p-5">
                  <h3 className="font-semibold"><Editable enabled={editEnabled} html={p.title} onChange={(v)=> setData((prev)=> ({...prev, projects: prev.projects.map(x=> x.id===p.id? {...x, title: v}: x)}))} /></h3>
                  <div className="text-sm text-slate-500 mt-1"><Editable enabled={editEnabled} html={p.location} onChange={(v)=> setData((prev)=> ({...prev, projects: prev.projects.map(x=> x.id===p.id? {...x, location: v}: x)}))} /></div>
                  <div className="flex flex-wrap gap-2 mt-3">
                    {p.tags.map((t, i)=> (
                      <span key={i} className="px-2 py-1 text-xs rounded-full bg-slate-100 border"><Editable enabled={editEnabled} html={t} onChange={(v)=> setData((prev)=> ({...prev, projects: prev.projects.map(x=> x.id===p.id? {...x, tags: x.tags.map((tt,ii)=> ii===i? v: tt)}: x)}))} /></span>
                    ))}
                  </div>
                  {editEnabled && (
                    <div className="mt-3 flex items-center gap-2">
                      <button className="px-3 py-1.5 text-xs rounded-full border" onClick={()=> setData((prev)=> ({...prev, projects: prev.projects.map(x=> x.id===p.id? {...x, tags: [...x.tags, "Tag"]}: x)}))}>+ tag</button>
                      <button className="px-3 py-1.5 text-xs rounded-full border" onClick={()=> rmProject(p.id)}>Supprimer</button>
                    </div>
                  )}
                </div>
              </div>
            ))}
          </div>
          {editEnabled && (
            <div className="mt-6">
              <button className="px-4 py-2 rounded-xl border" onClick={addProject}>+ Ajouter un projet</button>
            </div>
          )}
        </div>
      </section>

      {/* À PROPOS */}
      <section id="about" className="py-16 md:py-20 bg-slate-50">
        <div className="max-w-7xl mx-auto px-4">
          <h2 className="text-2xl md:text-3xl font-bold"><Editable enabled={editEnabled} html={data.about.title} onChange={(v)=> set("about.title", v)} /></h2>
          <p className="text-slate-700 mt-3 max-w-3xl"><Editable enabled={editEnabled} html={data.about.body} onChange={(v)=> set("about.body", v)} /></p>
          <div className="flex flex-wrap gap-2 mt-4">
            {data.about.badges.map((b, i)=> (
              <span key={i} className="px-3 py-1.5 rounded-full bg-white border shadow-sm text-sm"><Editable enabled={editEnabled} html={b} onChange={(v)=> setData((p)=> ({...p, about: { ...p.about, badges: p.about.badges.map((bb,ii)=> ii===i? v: bb) }}))} /></span>
            ))}
            {editEnabled && (
              <button className="px-3 py-1.5 rounded-full border" onClick={()=> setData((p)=> ({...p, about: { ...p.about, badges: [...p.about.badges, "Nouvelle mention"]}}))}>+ badge</button>
            )}
          </div>
        </div>
      </section>

      {/* FAQ */}
      <section id="faq" className="py-16 md:py-20">
        <div className="max-w-5xl mx-auto px-4">
          <h2 className="text-2xl md:text-3xl font-bold">Questions fréquentes</h2>
          <div className="mt-6 space-y-4">
            {data.faq.map((f)=> (
              <details key={f.id} className="group border rounded-xl p-5 bg-white">
                <summary className="font-medium cursor-pointer flex items-center justify-between list-none">
                  <Editable enabled={editEnabled} html={f.q} onChange={(v)=> setData((p)=> ({...p, faq: p.faq.map(x=> x.id===f.id? {...x, q: v}: x)}))} />
                </summary>
                <div className="text-slate-600 mt-3"><Editable enabled={editEnabled} html={f.a} onChange={(v)=> setData((p)=> ({...p, faq: p.faq.map(x=> x.id===f.id? {...x, a: v}: x)}))} /></div>
                {editEnabled && (
                  <div className="mt-3">
                    <button className="px-3 py-1.5 text-xs rounded-full border" onClick={()=> setData((p)=> ({...p, faq: p.faq.filter(x=> x.id!==f.id)}))}>Supprimer</button>
                  </div>
                )}
              </details>
            ))}
            {editEnabled && (
              <button className="px-4 py-2 rounded-xl border" onClick={()=> setData((p)=> ({...p, faq: [...p.faq, { id: uid(), q: "Nouvelle question?", a: "Réponse..." }]}))}>+ Question</button>
            )}
          </div>
        </div>
      </section>

      {/* CONTACT */}
      <section id="contact" className="py-16 md:py-20 bg-gradient-to-b from-slate-50 to-white border-t">
        <div className="max-w-7xl mx-auto px-4 grid md:grid-cols-2 gap-10 items-start">
          <div>
            <h2 className="text-2xl md:text-3xl font-bold">Obtenir une soumission</h2>
            <p className="text-slate-600 mt-2">Décrivez votre projet, joignez quelques photos (liens) et vos disponibilités.</p>
            <form
              onSubmit={(e)=> {
                e.preventDefault();
                const fd = new FormData(e.currentTarget);
                const msg = `Projet: ${fd.get('projet')}\nAdresse: ${fd.get('adresse')}\nTéléphone: ${fd.get('tel')}\nCourriel: ${fd.get('email')}\nLiens photos: ${fd.get('photos')}\nDétails: ${fd.get('details')}`;
                const mailto = `mailto:${data.contact.email}?subject=Soumission%20-${encodeURIComponent(fd.get('projet')||'Projet')}&body=${encodeURIComponent(msg)}`;
                window.location.href = mailto;
              }}
              className="mt-6 space-y-4"
            >
              <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                <input name="projet" required placeholder="Type de projet (ex: drain, pavé)" className="input" />
                <input name="adresse" placeholder="Adresse des travaux" className="input" />
                <input name="tel" placeholder="Téléphone" className="input" />
                <input name="email" type="email" required placeholder="Courriel" className="input" />
              </div>
              <input name="photos" placeholder="Liens vers des photos (Drive, iCloud, etc.)" className="input" />
              <textarea name="details" rows={5} placeholder="Décrivez les travaux, dimensions, délais, budget..." className="input" />
              <button type="submit" className="btn-primary inline-flex items-center gap-2"><Mail className="w-4 h-4" /> Envoyer</button>
            </form>
          </div>

          <div className="rounded-2xl border p-6 bg-white shadow-sm">
            <h3 className="font-semibold text-lg">Coordonnées</h3>
            <div className="mt-4 space-y-3 text-sm">
              <div className="flex items-center gap-2"><Phone className="w-4 h-4" /> <span>{data.contact.phone}</span></div>
              <div className="flex items-center gap-2"><Mail className="w-4 h-4" /> <span>{data.contact.email}</span></div>
              <div className="flex items-center gap-2"><MapPin className="w-4 h-4" /> <a className="underline" href={data.contact.mapLink} target="_blank">{data.contact.address}</a></div>
              <div className="flex items-center gap-2"><Building2 className="w-4 h-4" /> <span>{data.contact.hours}</span></div>
            </div>

            {editEnabled && (
              <div className="mt-4 space-y-2">
                <input className="input" value={data.contact.phone} onChange={(e)=> set("contact.phone", e.target.value)} />
                <input className="input" value={data.contact.email} onChange={(e)=> set("contact.email", e.target.value)} />
                <input className="input" value={data.contact.address} onChange={(e)=> set("contact.address", e.target.value)} />
                <input className="input" value={data.contact.mapLink} onChange={(e)=> set("contact.mapLink", e.target.value)} />
                <input className="input" value={data.contact.hours} onChange={(e)=> set("contact.hours", e.target.value)} />
              </div>
            )}

            <div className="mt-6">
              <img src={data.brand.logoUrl} alt="logo" className="w-full h-40 object-cover rounded-xl border" />
              {editEnabled && (
                <div className="mt-2 flex items-center gap-2">
                  <ImageIcon className="w-4 h-4" />
                  <input className="input" value={data.brand.logoUrl} onChange={(e)=> set("brand.logoUrl", e.target.value)} />
                </div>
              )}
            </div>
          </div>
        </div>
      </section>

      {/* FOOTER */}
      <footer className="py-10 border-t bg-slate-50">
        <div className="max-w-7xl mx-auto px-4 flex flex-col md:flex-row items-center justify-between gap-4">
          <div className="text-sm text-slate-600">{data.footer.legal}</div>
          <div className="flex items-center gap-4 text-sm">
            {data.footer.links.map((l)=> (
              <a key={l.id} href={l.href} className="hover:underline">{l.label}</a>
            ))}
          </div>
        </div>
      </footer>

      {/* Panneau Admin */}
      {adminAsk && (
        <div className="fixed inset-0 bg-black/40 backdrop-blur-sm z-[60] flex items-center justify-center p-4">
          <div className="w-full max-w-sm bg-white rounded-2xl p-6 border shadow-xl">
            <h3 className="font-semibold text-lg flex items-center gap-2"><Settings2 className="w-5 h-5" /> Accès Admin</h3>
            <p className="text-sm text-slate-600 mt-1">Entrez le mot de passe pour activer l'édition.</p>
            <input autoFocus type="password" className="input mt-4" value={pwdInput} onChange={(e)=> setPwdInput(e.target.value)} placeholder="Mot de passe (défaut: gmx)" />
            <div className="mt-4 flex items-center justify-end gap-2">
              <button className="px-4 py-2 rounded-lg border" onClick={()=> setAdminAsk(false)}>Annuler</button>
              <button className="px-4 py-2 rounded-lg bg-[var(--brand)] text-white" onClick={tryLogin}>Entrer</button>
            </div>
          </div>
        </div>
      )}

      {adminMode && (
        <div className="fixed bottom-4 right-4 z-[55] w-[min(92vw,480px)]">
          <div className="rounded-2xl border bg-white shadow-2xl overflow-hidden">
            <div className="flex items-center justify-between px-4 py-3 border-b">
              <div className="font-semibold text-sm">Panneau d'administration</div>
              <div className="flex items-center gap-2 text-xs">
                <label className="inline-flex items-center gap-2 px-3 py-1.5 rounded-full border bg-white">
                  <input type="checkbox" checked={editEnabled} onChange={(e)=> setEditEnabled(e.target.checked)} /> Édition
                </label>
                <button onClick={saveLS} className="px-3 py-1.5 rounded-full border">Sauvegarder</button>
              </div>
            </div>

            <div className="px-4 pt-3">
              <div className="flex items-center gap-2 mb-3 text-sm overflow-x-auto">
                {['contenu','marque','sauvegardes','reglages'].map(t=> (
                  <button key={t} onClick={()=> setActiveTab(t)} className={cx("px-3 py-1.5 rounded-full border", activeTab===t? "bg-[var(--brand)] text-white border-transparent" : "bg-white")}>{t[0].toUpperCase()+t.slice(1)}</button>
                ))}
              </div>

              {activeTab==='contenu' && (
                <div className="space-y-4 pb-4">
                  <div className="text-sm text-slate-600">Ajoutez/retirez des éléments dans Services, Projets et FAQ directement sur la page lorsque l'édition est activée.</div>
                </div>
              )}

              {activeTab==='marque' && (
                <div className="space-y-3 pb-4">
                  <label className="block text-sm">Nom de l'entreprise
                    <input className="input mt-1" value={data.brand.name} onChange={(e)=> set("brand.name", e.target.value)} />
                  </label>
                  <label className="block text-sm">Slogan
                    <input className="input mt-1" value={data.brand.tagline} onChange={(e)=> set("brand.tagline", e.target.value)} />
                  </label>
                  <label className="block text-sm">Couleur primaire
                    <input type="color" className="input mt-1 h-10" value={data.brand.primaryColor} onChange={(e)=> set("brand.primaryColor", e.target.value)} />
                  </label>
                  <label className="block text-sm">Logo (URL)
                    <input className="input mt-1" value={data.brand.logoUrl} onChange={(e)=> set("brand.logoUrl", e.target.value)} />
                  </label>
                </div>
              )}

              {activeTab==='sauvegardes' && (
                <div className="space-y-3 pb-4">
                  <div className="flex items-center gap-2">
                    <button onClick={exportJSON} className="px-3 py-1.5 rounded-full border inline-flex items-center gap-2"><Download className="w-4 h-4" /> Exporter JSON</button>
                    <button onClick={importJSON} className="px-3 py-1.5 rounded-full border inline-flex items-center gap-2"><Upload className="w-4 h-4" /> Importer JSON</button>
                    <button onClick={clearLS} className="px-3 py-1.5 rounded-full border">Effacer ce navigateur</button>
                  </div>
                  <textarea className="w-full h-40 border rounded-xl p-3 font-mono text-xs" value={jsonText} onChange={(e)=> setJsonText(e.target.value)} placeholder="Collez ici le JSON exporté pour l'import, ou cliquez sur Exporter pour l'afficher." />
                </div>
              )}

              {activeTab==='reglages' && (
                <div className="space-y-3 pb-4">
                  <label className="block text-sm">Mot de passe Admin
                    <input type="password" className="input mt-1" value={data.admin.password} onChange={(e)=> set("admin.password", e.target.value)} />
                  </label>
                  <div className="text-xs text-slate-500">Astuce: notez-le dans un endroit sûr. Le mot de passe n'est pas chiffré (usage local). Pour un site multi-utilisateurs, optez pour un CMS.</div>
                </div>
              )}
            </div>
          </div>
        </div>
      )}

      {/* Styles utilitaires */}
      <style>{`
        .btn-primary { background: var(--brand); color: white; padding: 0.5rem 0.9rem; border-radius: 9999px; font-weight: 600; }
        .input { width: 100%; border: 1px solid rgb(226,232,240); border-radius: 0.75rem; padding: 0.625rem 0.875rem; }
      `}</style>
    </div>
  );
}
