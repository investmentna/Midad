import React, { useState, useEffect, useMemo, useCallback, useRef } from 'react';
import { Search, PenSquare, Bookmark, BookmarkCheck, ArrowRight, X, MessageCircle, Share2 } from 'lucide-react';


function formatDate(iso) {
  const d = new Date(iso);
  if (isNaN(d)) return '';
  return `${d.getDate()} ${ARABIC_MONTHS[d.getMonth()]} ${d.getFullYear()}`;
}

function wordCount(text) {
  return (text || '').trim().split(/\s+/).filter(Boolean).length;
}

function readTimeMinutes(body) {
  return Math.max(1, Math.round(wordCount(body) / 180));
}

function readTimeLabel(n) {
  if (n <= 1) return 'دقيقة واحدة للقراءة';
  if (n === 2) return 'دقيقتان للقراءة';
  if (n <= 10) return `${n} دقائق للقراءة`;
  return `${n} دقيقة للقراءة`;
}

function initials(name) {
  if (!name) return '؟';
  return name.trim().charAt(0);
}

let uidCounter = 0;
function uid(prefix) {
  uidCounter += 1;
  return `${prefix}-${Date.now()}-${uidCounter}`;
}

/* ---------------------------------------------------------------------- */
/*  محلّل ماركداون مبسّط                                                   */
/* ---------------------------------------------------------------------- */

function parseInline(text, keyPrefix) {
  const nodes = [];
  let remaining = text;
  let key = 0;
  const regex = /(\[([^\]]+)\]\(([^)]+)\))|(\*\*([^*]+)\*\*)|(\*([^*]+)\*)|(`([^`]+)`)/;
  let guard = 0;
  while (remaining.length && guard < 500) {
    guard += 1;
    const m = remaining.match(regex);
    if (!m) { nodes.push(remaining); break; }
    const idx = m.index;
    if (idx > 0) nodes.push(remaining.slice(0, idx));
    if (m[1]) {
      nodes.push(<a key={`${keyPrefix}-${key++}`} href={m[3]} target="_blank" rel="noreferrer" className="inline-link">{m[2]}</a>);
    } else if (m[4]) {
      nodes.push(<strong key={`${keyPrefix}-${key++}`}>{m[5]}</strong>);
    } else if (m[6]) {
      nodes.push(<em key={`${keyPrefix}-${key++}`}>{m[7]}</em>);
    } else if (m[8]) {
      nodes.push(<code key={`${keyPrefix}-${key++}`} className="inline-code">{m[9]}</code>);
    }
    remaining = remaining.slice(idx + m[0].length);
  }
  return nodes;
}

function renderBody(markdown) {
  const blocks = (markdown || '').split(/\n\s*\n/);
  return blocks.map((block, i) => {
    const trimmed = block.trim();
    if (!trimmed) return null;
    if (trimmed.startsWith('### ')) return <h3 key={i} className="body-h3">{parseInline(trimmed.slice(4), `h3-${i}`)}</h3>;
    if (trimmed.startsWith('## ')) return <h2 key={i} className="body-h2">{parseInline(trimmed.slice(3), `h2-${i}`)}</h2>;
    if (trimmed.startsWith('> ')) {
      const clean = trimmed.split('\n').map(l => l.replace(/^>\s?/, '')).join(' ');
      return <blockquote key={i} className="body-quote">{parseInline(clean, `q-${i}`)}</blockquote>;
    }
    const imgMatch = trimmed.match(/^!\[([^\]]*)\]\(([^)]+)\)$/);
    if (imgMatch) {
      return <figure key={i} className="body-figure"><img src={imgMatch[2]} alt={imgMatch[1]} /></figure>;
    }
    if (/^[-*]\s/.test(trimmed)) {
      const items = trimmed.split('\n').filter(l => l.trim()).map(l => l.replace(/^[-*]\s/, ''));
      return <ul key={i} className="body-list">{items.map((it, j) => <li key={j}>{parseInline(it, `li-${i}-${j}`)}</li>)}</ul>;
    }
    return <p key={i} className="body-p">{parseInline(trimmed, `p-${i}`)}</p>;
  });
}

/* ---------------------------------------------------------------------- */
/*  مكونات فرعية                                                          */
/* ---------------------------------------------------------------------- */

function Avatar({ name, size = 36 }) {
  return (
    <div className="avatar" style={{ width: size, height: size, fontSize: size * 0.4 }}>
      {initials(name)}
    </div>
  );
}

function TagPill({ label, active, onClick }) {
  return (
    <button type="button" className={`tag-pill${active ? ' tag-pill--active' : ''}`} onClick={onClick}>
      {label}
    </button>
  );
}

function ArticleRow({ article, onOpen }) {
  const rt = readTimeMinutes(article.body);
  return (
    <article className="row-card" onClick={() => onOpen(article.id)}>
      <div className="row-card__text">
        <div className="row-card__tag">{article.tags[0]}</div>
        <h3 className="row-card__title">{article.title}</h3>
        <p className="row-card__subtitle">{article.subtitle}</p>
        <div className="row-card__meta">
          <span>{article.author}</span>
          <span className="dot-sep" />
          <span>{formatDate(article.createdAt)}</span>
          <span className="dot-sep" />
          <span>{readTimeLabel(rt)}</span>
        </div>
      </div>
      <div className="row-card__thumb">
        <img src={article.cover} alt="" loading="lazy" />
      </div>
    </article>
  );
}

function Toast({ message }) {
  if (!message) return null;
  return <div className="toast">{message}</div>;
}

/* ---------------------------------------------------------------------- */
/*  التطبيق الرئيسي                                                       */
/* ---------------------------------------------------------------------- */

export default function App() {
  const [loading, setLoading] = useState(true);
  const [articles, setArticles] = useState([]);
  const [profile, setProfile] = useState(null);
  const [bookmarks, setBookmarks] = useState([]);
  const [myClaps, setMyClaps] = useState({});

  const [view, setView] = useState('home'); // home | article | editor | bookmarks
  const [currentId, setCurrentId] = useState(null);
  const [query, setQuery] = useState('');
  const [activeTag, setActiveTag] = useState('الكل');
  const [sortMode, setSortMode] = useState('latest'); // latest | trending

  const [showProfileModal, setShowProfileModal] = useState(false);
  const [pendingAction, setPendingAction] = useState(null); // 'publish' | 'comment'
  const [toast, setToast] = useState('');
  const toastTimer = useRef(null);

  const [commentDrafts, setCommentDrafts] = useState({});

  const [draft, setDraft] = useState({ title: '', subtitle: '', cover: '', tagsText: '', body: '' });
  const [showPreview, setShowPreview] = useState(false);

  const showToast = useCallback((msg) => {
    setToast(msg);
    if (toastTimer.current) clearTimeout(toastTimer.current);
    toastTimer.current = setTimeout(() => setToast(''), 2400);
  }, []);

  /* ---- تحميل البيانات ---- */
  useEffect(() => {
    (async () => {
      try {
        const res = await window.storage.get('midad-articles', true);
        const parsed = JSON.parse(res.value);
        setArticles(Array.isArray(parsed) && parsed.length ? parsed : SEED_ARTICLES);
      } catch (e) {
        try { await window.storage.set('midad-articles', JSON.stringify(SEED_ARTICLES), true); } catch (e2) { /* ignore */ }
        setArticles(SEED_ARTICLES);
      }
      try {
        const p = await window.storage.get('midad-profile', false);
        setProfile(JSON.parse(p.value));
      } catch (e) { setProfile(null); }
      try {
        const b = await window.storage.get('midad-bookmarks', false);
        const parsed = JSON.parse(b.value);
        setBookmarks(Array.isArray(parsed) ? parsed : []);
      } catch (e) { setBookmarks([]); }
      try {
        const c = await window.storage.get('midad-claps', false);
        const parsed = JSON.parse(c.value);
        setMyClaps(parsed && typeof parsed === 'object' ? parsed : {});
      } catch (e) { setMyClaps({}); }
      setLoading(false);
    })();
  }, []);

  /* ---- دوال الحفظ ---- */
  const persistArticles = useCallback(async (next) => {
    setArticles(next);
    try { await window.storage.set('midad-articles', JSON.stringify(next), true); } catch (e) { showToast('تعذّر الحفظ، حاول مجددًا'); }
  }, [showToast]);

  const persistProfile = useCallback(async (next) => {
    setProfile(next);
    try { await window.storage.set('midad-profile', JSON.stringify(next), false); } catch (e) { /* ignore */ }
  }, []);

  const persistBookmarks = useCallback(async (next) => {
    setBookmarks(next);
    try { await window.storage.set('midad-bookmarks', JSON.stringify(next), false); } catch (e) { /* ignore */ }
  }, []);

  const persistClaps = useCallback(async (next) => {
    setMyClaps(next);
    try { await window.storage.set('midad-claps', JSON.stringify(next), false); } catch (e) { /* ignore */ }
  }, []);

  /* ---- منطق العرض ---- */
  const filteredArticles = useMemo(() => {
    let list = [...articles];
    if (activeTag !== 'الكل') list = list.filter(a => a.tags.includes(activeTag));
    if (query.trim()) {
      const q = query.trim().toLowerCase();
      list = list.filter(a =>
        a.title.toLowerCase().includes(q) ||
        (a.subtitle || '').toLowerCase().includes(q) ||
        a.author.toLowerCase().includes(q) ||
        a.tags.some(t => t.toLowerCase().includes(q))
      );
    }
    if (sortMode === 'trending') list.sort((a, b) => (b.claps || 0) - (a.claps || 0));
    else list.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
    return list;
  }, [articles, activeTag, query, sortMode]);

  const currentArticle = useMemo(() => articles.find(a => a.id === currentId) || null, [articles, currentId]);
  const bookmarkedArticles = useMemo(() => articles.filter(a => bookmarks.includes(a.id)), [articles, bookmarks]);

  /* ---- أفعال ---- */
  const openArticle = (id) => { setCurrentId(id); setView('article'); window.scrollTo?.(0, 0); };

  const requireProfile = (action) => {
    if (!profile || !profile.name || !profile.name.trim()) {
      setPendingAction(action);
      setShowProfileModal(true);
      return false;
    }
    return true;
  };

  const saveProfileForm = (name, bio) => {
    const next = { name: name.trim(), bio: (bio || '').trim() };
    if (!next.name) return;
    persistProfile(next);
    setShowProfileModal(false);
    if (pendingAction === 'publish') { setView('editor'); }
    setPendingAction(null);
  };

  const handleClap = (articleId) => {
    const current = myClaps[articleId] || 0;
    if (current >= 20) { showToast('وصلت للحد الأقصى من التصفيق لهذا المقال'); return; }
    persistClaps({ ...myClaps, [articleId]: current + 1 });
    persistArticles(articles.map(a => a.id === articleId ? { ...a, claps: (a.claps || 0) + 1 } : a));
  };

  const toggleBookmark = (id) => {
    const has = bookmarks.includes(id);
    persistBookmarks(has ? bookmarks.filter(x => x !== id) : [...bookmarks, id]);
    showToast(has ? 'تمت الإزالة من المحفوظات' : 'تم الحفظ في المحفوظات');
  };

  const shareArticle = async (article) => {
    const fakeUrl = `midad.app/story/${article.id}`;
    try { await navigator.clipboard.writeText(fakeUrl); showToast('تم نسخ رابط المقال'); }
    catch (e) { showToast(fakeUrl); }
  };

  const submitComment = (articleId) => {
    if (!requireProfile('comment')) return;
    const text = (commentDrafts[articleId] || '').trim();
    if (!text) return;
    const comment = { id: uid('c'), author: profile.name, text, createdAt: new Date().toISOString() };
    persistArticles(articles.map(a => a.id === articleId ? { ...a, comments: [...(a.comments || []), comment] } : a));
    setCommentDrafts({ ...commentDrafts, [articleId]: '' });
  };

  const startEditor = () => {
    if (!requireProfile('publish')) return;
    setDraft({ title: '', subtitle: '', cover: '', tagsText: '', body: '' });
    setShowPreview(false);
    setView('editor');
  };

  const publishDraft = () => {
    if (!requireProfile('publish')) return;
    if (!draft.title.trim() || !draft.body.trim()) { showToast('العنوان والمحتوى مطلوبان'); return; }
    const tags = draft.tagsText.split(/[،,]/).map(t => t.trim()).filter(Boolean);
    const article = {
      id: uid('a'),
      title: draft.title.trim(),
      subtitle: draft.subtitle.trim(),
      cover: draft.cover.trim() || `https://picsum.photos/seed/${uid('seed')}/1200/700`,
      tags: tags.length ? tags : ['عام'],
      author: profile.name,
      authorBio: profile.bio || '',
      createdAt: new Date().toISOString(),
      claps: 0,
      comments: []
    };
    article.body = draft.body;
    const next = [article, ...articles];
    persistArticles(next);
    showToast('تم نشر المقال');
    setCurrentId(article.id);
    setView('article');
  };

  const resetDemoData = async () => {
    const ok = window.confirm('سيؤدي هذا إلى حذف جميع المقالات المنشورة من الجميع (البيانات مشتركة بين مستخدمي هذه الأداة) واستبدالها بالمحتوى الافتراضي. هل تريد المتابعة؟');
    if (!ok) return;
    await persistArticles(SEED_ARTICLES);
    showToast('تمت إعادة تعيين البيانات التجريبية');
  };

  /* ---- شاشة التحميل ---- */
  if (loading) {
    return (
      <div dir="rtl" className="midad-app">
        <Styles />
        <div className="loading-screen">
          <div className="loading-mark">مِداد</div>
          <div className="loading-sub">جارٍ تحضير المنصة…</div>
        </div>
      </div>
    );
  }

  return (
    <div dir="rtl" className="midad-app">
      <Styles />

      {/* ---- شريط التنقل ---- */}
      <header className="nav">
        <div className="nav__inner">
          <button className="brand" onClick={() => setView('home')}>مِداد</button>

          <div className="nav__search">
            <Search size={16} className="nav__search-icon" />
            <input
              type="text"
              placeholder="ابحث عن مقالات أو كتّاب أو مواضيع"
              value={query}
              onChange={e => { setQuery(e.target.value); if (view !== 'home') setView('home'); }}
            />
          </div>

          <nav className="nav__actions">
            <button className="nav__write" onClick={startEditor}>
              <PenSquare size={16} />
              <span>اكتب</span>
            </button>
            <button className="nav__icon-btn" onClick={() => setView('bookmarks')} title="المحفوظات">
              <Bookmark size={18} />
            </button>
            <button className="nav__avatar-btn" onClick={() => { setPendingAction(null); setShowProfileModal(true); }} title="حسابك">
              <Avatar name={profile?.name} size={32} />
            </button>
          </nav>
        </div>
      </header>

      {/* ---- محتوى الصفحات ---- */}
      {view === 'home' && (
        <main className="page">
          <div className="tagbar">
            <TagPill label="الكل" active={activeTag === 'الكل'} onClick={() => setActiveTag('الكل')} />
            {ALL_TAGS.map(t => (
              <TagPill key={t} label={t} active={activeTag === t} onClick={() => setActiveTag(t)} />
            ))}
            <div className="tagbar__spacer" />
            <div className="sort-toggle">
              <button className={sortMode === 'latest' ? 'active' : ''} onClick={() => setSortMode('latest')}>الأحدث</button>
              <button className={sortMode === 'trending' ? 'active' : ''} onClick={() => setSortMode('trending')}>الأكثر تفاعلًا</button>
            </div>
          </div>

          {filteredArticles.length === 0 && (
            <div className="empty-state">
              <p>لا توجد مقالات مطابقة لبحثك.</p>
            </div>
          )}

          {filteredArticles.length > 0 && (
            <>
              <article className="hero-card" onClick={() => openArticle(filteredArticles[0].id)}>
                <div className="hero-card__image">
                  <img src={filteredArticles[0].cover} alt="" />
                </div>
                <div className="hero-card__text">
                  <div className="row-card__tag">{filteredArticles[0].tags[0]}</div>
                  <h2 className="hero-card__title">{filteredArticles[0].title}</h2>
                  <p className="hero-card__subtitle">{filteredArticles[0].subtitle}</p>
                  <div className="row-card__meta">
                    <Avatar name={filteredArticles[0].author} size={28} />
                    <span>{filteredArticles[0].author}</span>
                    <span className="dot-sep" />
                    <span>{formatDate(filteredArticles[0].createdAt)}</span>
                    <span className="dot-sep" />
                    <span>{readTimeLabel(readTimeMinutes(filteredArticles[0].body))}</span>
                  </div>
                </div>
              </article>

              <div className="row-list">
                {filteredArticles.slice(1).map(a => (
                  <ArticleRow key={a.id} article={a} onOpen={openArticle} />
                ))}
              </div>
            </>
          )}

          <footer className="footer">
            <p>مِداد منصة تجريبية للنشر — المقالات والتعليقات وعدد التصفيقات مشتركة بين كل من يفتح هذه الأداة، بينما ملفك الشخصي ومحفوظاتك خاصة بك وحدك.</p>
            <button className="footer__reset" onClick={resetDemoData}>إعادة تعيين البيانات التجريبية</button>
          </footer>
        </main>
      )}

      {view === 'bookmarks' && (
        <main className="page">
          <div className="page-header">
            <button className="back-btn" onClick={() => setView('home')}>
              <ArrowRight size={16} />
              <span>الرئيسية</span>
            </button>
            <h2 className="page-header__title">محفوظاتك</h2>
          </div>
          {bookmarkedArticles.length === 0 ? (
            <div className="empty-state">
              <p>لم تحفظ أي مقالات بعد.</p>
              <p className="empty-state__hint">اضغط على أيقونة الحفظ داخل أي مقال لإضافته هنا.</p>
            </div>
          ) : (
            <div className="row-list">
              {bookmarkedArticles.map(a => <ArticleRow key={a.id} article={a} onOpen={openArticle} />)}
            </div>
          )}
        </main>
      )}

      {view === 'article' && currentArticle && (
        <ArticlePage
          article={currentArticle}
          isBookmarked={bookmarks.includes(currentArticle.id)}
          myClapCount={myClaps[currentArticle.id] || 0}
          onBack={() => setView('home')}
          onClap={() => handleClap(currentArticle.id)}
          onBookmark={() => toggleBookmark(currentArticle.id)}
          onShare={() => shareArticle(currentArticle)}
          commentDraft={commentDrafts[currentArticle.id] || ''}
          onCommentChange={(v) => setCommentDrafts({ ...commentDrafts, [currentArticle.id]: v })}
          onCommentSubmit={() => submitComment(currentArticle.id)}
          onTagClick={(t) => { setActiveTag(t); setView('home'); }}
        />
      )}

      {view === 'editor' && (
        <EditorPage
          draft={draft}
          setDraft={setDraft}
          showPreview={showPreview}
          setShowPreview={setShowPreview}
          onCancel={() => setView('home')}
          onPublish={publishDraft}
        />
      )}

      {showProfileModal && (
        <ProfileModal
          initialName={profile?.name || ''}
          initialBio={profile?.bio || ''}
          onClose={() => { setShowProfileModal(false); setPendingAction(null); }}
          onSave={saveProfileForm}
          hasProfile={!!(profile && profile.name)}
        />
      )}

      <Toast message={toast} />
    </div>
  );
}

/* ---------------------------------------------------------------------- */
/*  صفحة المقال                                                           */
/* ---------------------------------------------------------------------- */

function ArticlePage({ article, isBookmarked, myClapCount, onBack, onClap, onBookmark, onShare, commentDraft, onCommentChange, onCommentSubmit, onTagClick }) {
  const rt = readTimeMinutes(article.body);
  return (
    <main className="page page--article">
      <button className="back-btn back-btn--top" onClick={onBack}>
        <ArrowRight size={16} />
        <span>الرئيسية</span>
      </button>

      <div className="article-cover">
        <img src={article.cover} alt="" />
      </div>

      <div className="article-body">
        <div className="article-tags">
          {article.tags.map(t => (
            <button key={t} className="tag-pill" onClick={() => onTagClick(t)}>{t}</button>
          ))}
        </div>

        <h1 className="article-title">{article.title}</h1>
        {article.subtitle && <p className="article-subtitle">{article.subtitle}</p>}

        <div className="article-meta">
          <Avatar name={article.author} size={40} />
          <div className="article-meta__text">
            <div className="article-meta__author">{article.author}</div>
            <div className="article-meta__sub">{formatDate(article.createdAt)} <span className="dot-sep" /> {readTimeLabel(rt)}</div>
          </div>
          <div className="article-meta__actions">
            <button className={`icon-action${isBookmarked ? ' icon-action--active' : ''}`} onClick={onBookmark} title="حفظ">
              {isBookmarked ? <BookmarkCheck size={18} /> : <Bookmark size={18} />}
            </button>
            <button className="icon-action" onClick={onShare} title="مشاركة">
              <Share2 size={18} />
            </button>
          </div>
        </div>

        <div className="article-content">
          {renderBody(article.body)}
        </div>

        <div className="clap-section">
          <button className={`clap-btn${myClapCount > 0 ? ' clap-btn--active' : ''}`} onClick={onClap}>
            <span className="clap-btn__emoji" aria-hidden="true">👏</span>
            <span>{article.claps || 0}</span>
          </button>
          <span className="clap-section__hint">
            {myClapCount > 0 ? `صفّقت له ${myClapCount} ${myClapCount === 1 ? 'مرة' : 'مرات'}` : 'كن أول من يصفّق لهذا المقال'}
          </span>
        </div>

        {article.authorBio && (
          <div className="author-card">
            <Avatar name={article.author} size={48} />
            <div>
              <div className="author-card__name">{article.author}</div>
              <div className="author-card__bio">{article.authorBio}</div>
            </div>
          </div>
        )}

        <div className="comments-section">
          <h3 className="comments-section__title">
            <MessageCircle size={18} />
            <span>{(article.comments || []).length} تعليق</span>
          </h3>

          <div className="comment-form">
            <textarea
              placeholder="أضف تعليقًا..."
              value={commentDraft}
              onChange={e => onCommentChange(e.target.value)}
              rows={2}
            />
            <button onClick={onCommentSubmit} disabled={!commentDraft.trim()}>إرسال</button>
          </div>

          <div className="comment-list">
            {(article.comments || []).slice().reverse().map(c => (
              <div key={c.id} className="comment">
                <Avatar name={c.author} size={32} />
                <div className="comment__body">
                  <div className="comment__meta">
                    <span className="comment__author">{c.author}</span>
                    <span className="dot-sep" />
                    <span className="comment__date">{formatDate(c.createdAt)}</span>
                  </div>
                  <p className="comment__text">{c.text}</p>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </main>
  );
}

/* ---------------------------------------------------------------------- */
/*  صفحة الكتابة والنشر                                                   */
/* ---------------------------------------------------------------------- */

function EditorPage({ draft, setDraft, showPreview, setShowPreview, onCancel, onPublish }) {
  const tags = draft.tagsText.split(/[،,]/).map(t => t.trim()).filter(Boolean);
  return (
    <main className="page page--editor">
      <div className="editor-topbar">
        <button className="back-btn" onClick={onCancel}>
          <ArrowRight size={16} />
          <span>إلغاء</span>
        </button>
        <div className="editor-topbar__actions">
          <button className="ghost-btn" onClick={() => setShowPreview(!showPreview)}>
            {showPreview ? 'تحرير' : 'معاينة'}
          </button>
          <button className="publish-btn" onClick={onPublish}>نشر المقال</button>
        </div>
      </div>

      {!showPreview ? (
        <div className="editor-form">
          <input
            className="editor-title-input"
            placeholder="العنوان"
            value={draft.title}
            onChange={e => setDraft({ ...draft, title: e.target.value })}
          />
          <input
            className="editor-subtitle-input"
            placeholder="عنوان فرعي (اختياري)"
            value={draft.subtitle}
            onChange={e => setDraft({ ...draft, subtitle: e.target.value })}
          />
          <input
            className="editor-field-input"
            placeholder="رابط صورة الغلاف (اختياري)"
            value={draft.cover}
            onChange={e => setDraft({ ...draft, cover: e.target.value })}
          />
          {draft.cover && (
            <div className="editor-cover-preview"><img src={draft.cover} alt="" /></div>
          )}
          <input
            className="editor-field-input"
            placeholder="أضف وسومًا مفصولة بفواصل، مثل: تقنية، تصميم"
            value={draft.tagsText}
            onChange={e => setDraft({ ...draft, tagsText: e.target.value })}
          />
          {tags.length > 0 && (
            <div className="editor-tags-preview">
              {tags.map(t => <span key={t} className="tag-pill">{t}</span>)}
            </div>
          )}
          <textarea
            className="editor-body-input"
            placeholder="ابدأ الكتابة… يمكنك استخدام ## للعناوين، **نص عريض**، *نص مائل*، > للاقتباس، وعلامة - لبدء قائمة."
            value={draft.body}
            onChange={e => setDraft({ ...draft, body: e.target.value })}
            rows={16}
          />
        </div>
      ) : (
        <div className="article-body article-body--preview">
          <h1 className="article-title">{draft.title || 'بلا عنوان'}</h1>
          {draft.subtitle && <p className="article-subtitle">{draft.subtitle}</p>}
          <div className="article-content">{renderBody(draft.body)}</div>
        </div>
      )}
    </main>
  );
}

/* ---------------------------------------------------------------------- */
/*  نافذة الملف الشخصي                                                     */
/* ---------------------------------------------------------------------- */

function ProfileModal({ initialName, initialBio, onClose, onSave, hasProfile }) {
  const [name, setName] = useState(initialName);
  const [bio, setBio] = useState(initialBio);
  return (
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal" onClick={e => e.stopPropagation()}>
        <div className="modal__header">
          <h3>{hasProfile ? 'حسابك' : 'قبل أن تبدأ'}</h3>
          <button className="icon-action" onClick={onClose}><X size={18} /></button>
        </div>
        <p className="modal__desc">
          {hasProfile ? 'يمكنك تحديث اسمك ونبذتك في أي وقت.' : 'أضف اسمك حتى يظهر على مقالاتك وتعليقاتك.'}
        </p>
        <label className="modal__label">الاسم</label>
        <input className="editor-field-input" value={name} onChange={e => setName(e.target.value)} placeholder="اسمك" />
        <label className="modal__label">نبذة قصيرة (اختياري)</label>
        <textarea className="editor-field-input" value={bio} onChange={e => setBio(e.target.value)} rows={3} placeholder="اكتب سطرًا أو سطرين عن نفسك" />
        <button className="publish-btn modal__save" disabled={!name.trim()} onClick={() => onSave(name, bio)}>حفظ ومتابعة</button>
      </div>
    </div>
  );
}

/* ---------------------------------------------------------------------- */
/*  الأنماط                                                                */
/* ---------------------------------------------------------------------- */

function Styles() {
  return (
    <style>{`
      @import url('https://fonts.googleapis.com/css2?family=Noto+Naskh+Arabic:wght@400;500;600;700&family=IBM+Plex+Sans+Arabic:wght@400;500;600&display=swap');

      .midad-app {
        --paper: #F3F0E7;
        --surface: #FFFFFF;
        --ink: #1E1C16;
        --ink-muted: #6B6455;
        --line: #DEDACB;
        --accent: #1F4D3D;
        --accent-2: #A9782F;
        --danger: #9C3B2E;
        font-family: 'IBM Plex Sans Arabic', system-ui, sans-serif;
        background: var(--paper);
        color: var(--ink);
        min-height: 100vh;
        line-height: 1.6;
      }
      .midad-app * { box-sizing: border-box; }
      .midad-app img { max-width: 100%; display: block; }
      .midad-app button { font-family: inherit; cursor: pointer; background: none; border: none; color: inherit; }
      .midad-app input, .midad-app textarea { font-family: inherit; }

      .loading-screen { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 8px; }
      .loading-mark { font-family: 'Noto Naskh Arabic', serif; font-size: 40px; color: var(--accent); }
      .loading-sub { color: var(--ink-muted); font-size: 14px; }

      /* --- نافبار --- */
      .nav { position: sticky; top: 0; z-index: 30; background: rgba(243,240,231,0.92); backdrop-filter: blur(6px); border-bottom: 1px solid var(--line); }
      .nav__inner { max-width: 1040px; margin: 0 auto; padding: 14px 20px; display: flex; align-items: center; gap: 20px; }
      .brand { font-family: 'Noto Naskh Arabic', serif; font-size: 24px; font-weight: 700; color: var(--accent); flex-shrink: 0; }
      .nav__search { flex: 1; display: flex; align-items: center; gap: 8px; background: var(--surface); border: 1px solid var(--line); padding: 8px 14px; max-width: 420px; }
      .nav__search input { flex: 1; border: none; outline: none; background: transparent; font-size: 14px; color: var(--ink); }
      .nav__search-icon { color: var(--ink-muted); flex-shrink: 0; }
      .nav__actions { display: flex; align-items: center; gap: 10px; margin-inline-start: auto; }
      .nav__write { display: flex; align-items: center; gap: 6px; background: var(--accent); color: #FBF9F3; padding: 8px 16px; font-size: 14px; font-weight: 500; }
      .nav__icon-btn { padding: 6px; color: var(--ink); }
      .nav__avatar-btn { padding: 0; border-radius: 999px; }

      .avatar { border-radius: 999px; background: var(--accent); color: #FBF9F3; display: flex; align-items: center; justify-content: center; font-weight: 600; flex-shrink: 0; }

      /* --- شريط الوسوم --- */
      .page { max-width: 1040px; margin: 0 auto; padding: 28px 20px 60px; }
      .tagbar { display: flex; align-items: center; flex-wrap: wrap; gap: 8px; margin-bottom: 28px; }
      .tagbar__spacer { flex: 1; }
      .tag-pill { border: 1px solid var(--line); background: var(--surface); padding: 6px 14px; font-size: 13px; border-radius: 999px; color: var(--ink-muted); transition: border-color .15s, color .15s; }
      .tag-pill--active { border-color: var(--accent); color: var(--accent); font-weight: 600; }
      .sort-toggle { display: flex; gap: 2px; background: var(--surface); border: 1px solid var(--line); border-radius: 999px; padding: 3px; }
      .sort-toggle button { padding: 5px 12px; font-size: 12px; border-radius: 999px; color: var(--ink-muted); }
      .sort-toggle button.active { background: var(--ink); color: var(--paper); }

      .dot-sep { width: 3px; height: 3px; border-radius: 999px; background: var(--ink-muted); display: inline-block; margin: 0 2px; opacity: .7; }

      /* --- بطاقة رئيسية --- */
      .hero-card { display: grid; grid-template-columns: 1.1fr 1fr; gap: 32px; align-items: center; padding-bottom: 32px; border-bottom: 1px solid var(--line); margin-bottom: 32px; cursor: pointer; }
      .hero-card__image { overflow: hidden; aspect-ratio: 4/3; background: var(--line); }
      .hero-card__image img { width: 100%; height: 100%; object-fit: cover; }
      .hero-card__title { font-family: 'Noto Naskh Arabic', serif; font-size: 30px; line-height: 1.35; margin: 10px 0 8px; }
      .hero-card__subtitle { color: var(--ink-muted); font-size: 16px; margin-bottom: 14px; }
      .row-card__tag { font-size: 12px; color: var(--accent-2); font-weight: 600; }
      .row-card__meta { display: flex; align-items: center; gap: 6px; font-size: 13px; color: var(--ink-muted); }

      /* --- قائمة المقالات --- */
      .row-list { display: flex; flex-direction: column; }
      .row-card { display: flex; gap: 24px; padding: 24px 0; border-bottom: 1px solid var(--line); cursor: pointer; }
      .row-card__text { flex: 1; min-width: 0; }
      .row-card__title { font-family: 'Noto Naskh Arabic', serif; font-size: 21px; line-height: 1.4; margin: 6px 0 6px; }
      .row-card__subtitle { color: var(--ink-muted); font-size: 14.5px; margin-bottom: 10px; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
      .row-card__thumb { width: 128px; height: 96px; flex-shrink: 0; overflow: hidden; background: var(--line); }
      .row-card__thumb img { width: 100%; height: 100%; object-fit: cover; }

      .empty-state { text-align: center; padding: 60px 20px; color: var(--ink-muted); }
      .empty-state__hint { font-size: 13px; margin-top: 6px; }

      .footer { margin-top: 48px; padding-top: 24px; border-top: 1px solid var(--line); text-align: center; }
      .footer p { color: var(--ink-muted); font-size: 12.5px; max-width: 520px; margin: 0 auto 10px; }
      .footer__reset { font-size: 12.5px; color: var(--danger); text-decoration: underline; text-underline-offset: 3px; }

      /* --- رأس صفحة فرعية --- */
      .page-header { display: flex; align-items: center; gap: 18px; margin-bottom: 28px; }
      .page-header__title { font-family: 'Noto Naskh Arabic', serif; font-size: 24px; }
      .back-btn, .back-btn--top { display: inline-flex; align-items: center; gap: 6px; color: var(--ink-muted); font-size: 14px; }
      .back-btn--top { margin-bottom: 18px; }

      /* --- صفحة المقال --- */
      .page--article { max-width: 760px; }
      .article-cover { aspect-ratio: 16/8; overflow: hidden; background: var(--line); margin-bottom: 28px; }
      .article-cover img { width: 100%; height: 100%; object-fit: cover; }
      .article-tags { display: flex; gap: 8px; margin-bottom: 14px; }
      .article-title { font-family: 'Noto Naskh Arabic', serif; font-size: 36px; line-height: 1.4; margin-bottom: 12px; }
      .article-subtitle { font-size: 19px; color: var(--ink-muted); font-family: 'Noto Naskh Arabic', serif; font-style: italic; margin-bottom: 22px; }
      .article-meta { display: flex; align-items: center; gap: 12px; padding: 18px 0; border-top: 1px solid var(--line); border-bottom: 1px solid var(--line); margin-bottom: 32px; }
      .article-meta__text { flex: 1; }
      .article-meta__author { font-weight: 600; font-size: 14.5px; }
      .article-meta__sub { font-size: 13px; color: var(--ink-muted); }
      .article-meta__actions { display: flex; gap: 6px; }
      .icon-action { padding: 8px; border-radius: 999px; color: var(--ink-muted); }
      .icon-action:hover { background: var(--surface); color: var(--ink); }
      .icon-action--active { color: var(--accent); }

      .article-content { font-family: 'Noto Naskh Arabic', serif; font-size: 19px; line-height: 1.95; color: var(--ink); }
      .body-p { margin-bottom: 22px; }
      .body-h2 { font-size: 25px; margin: 36px 0 14px; }
      .body-h3 { font-size: 21px; margin: 28px 0 12px; }
      .body-quote { border-inline-start: 3px solid var(--accent); padding-inline-start: 18px; margin: 26px 0; font-style: italic; color: var(--ink-muted); font-size: 20px; }
      .body-list { margin: 0 0 22px; padding-inline-start: 22px; }
      .body-list li { margin-bottom: 8px; }
      .body-figure { margin: 26px 0; }
      .inline-link { color: var(--accent); text-decoration: underline; text-underline-offset: 3px; }
      .inline-code { background: var(--surface); border: 1px solid var(--line); padding: 1px 6px; font-size: 0.85em; font-family: monospace; }

      .clap-section { display: flex; align-items: center; gap: 14px; padding: 28px 0; margin: 20px 0 32px; border-top: 1px solid var(--line); border-bottom: 1px solid var(--line); }
      .clap-btn { display: flex; align-items: center; gap: 8px; border: 1px solid var(--line); background: var(--surface); padding: 10px 20px; border-radius: 999px; font-weight: 600; font-size: 15px; }
      .clap-btn--active { border-color: var(--accent); color: var(--accent); }
      .clap-btn__emoji { font-size: 18px; }
      .clap-section__hint { font-size: 13px; color: var(--ink-muted); }

      .author-card { display: flex; gap: 14px; align-items: flex-start; background: var(--surface); border: 1px solid var(--line); padding: 20px; margin-bottom: 36px; }
      .author-card__name { font-weight: 600; margin-bottom: 4px; }
      .author-card__bio { font-size: 14px; color: var(--ink-muted); }

      .comments-section__title { display: flex; align-items: center; gap: 8px; font-size: 17px; margin-bottom: 18px; }
      .comment-form { display: flex; flex-direction: column; gap: 10px; margin-bottom: 28px; }
      .comment-form textarea { border: 1px solid var(--line); background: var(--surface); padding: 12px 14px; font-size: 14.5px; resize: vertical; outline: none; }
      .comment-form button { align-self: flex-end; background: var(--ink); color: var(--paper); padding: 8px 20px; font-size: 13.5px; font-weight: 500; }
      .comment-form button:disabled { opacity: .4; cursor: default; }
      .comment-list { display: flex; flex-direction: column; gap: 20px; }
      .comment { display: flex; gap: 12px; }
      .comment__body { flex: 1; }
      .comment__meta { display: flex; align-items: center; gap: 6px; font-size: 13px; margin-bottom: 4px; }
      .comment__author { font-weight: 600; }
      .comment__date { color: var(--ink-muted); }
      .comment__text { font-size: 14.5px; color: var(--ink); }

      /* --- المحرر --- */
      .page--editor { max-width: 720px; }
      .editor-topbar { display: flex; align-items: center; justify-content: space-between; margin-bottom: 28px; }
      .editor-topbar__actions { display: flex; align-items: center; gap: 10px; }
      .ghost-btn { border: 1px solid var(--line); padding: 8px 16px; font-size: 13.5px; }
      .publish-btn { background: var(--accent); color: #FBF9F3; padding: 9px 20px; font-size: 13.5px; font-weight: 600; }
      .publish-btn:disabled { opacity: .5; }

      .editor-form { display: flex; flex-direction: column; gap: 14px; }
      .editor-title-input { border: none; background: transparent; font-family: 'Noto Naskh Arabic', serif; font-size: 32px; outline: none; padding: 4px 0; }
      .editor-title-input::placeholder { color: #B7AF9C; }
      .editor-subtitle-input { border: none; background: transparent; font-family: 'Noto Naskh Arabic', serif; font-style: italic; font-size: 18px; color: var(--ink-muted); outline: none; padding-bottom: 12px; border-bottom: 1px solid var(--line); }
      .editor-field-input { border: 1px solid var(--line); background: var(--surface); padding: 10px 14px; font-size: 14px; outline: none; }
      .editor-cover-preview { aspect-ratio: 16/7; overflow: hidden; background: var(--line); }
      .editor-cover-preview img { width: 100%; height: 100%; object-fit: cover; }
      .editor-tags-preview { display: flex; gap: 6px; flex-wrap: wrap; }
      .editor-body-input { border: 1px solid var(--line); background: var(--surface); padding: 16px; font-size: 17px; line-height: 1.8; font-family: 'Noto Naskh Arabic', serif; outline: none; resize: vertical; }

      /* --- نافذة الملف الشخصي --- */
      .modal-overlay { position: fixed; inset: 0; background: rgba(30,28,22,0.5); display: flex; align-items: center; justify-content: center; z-index: 50; padding: 20px; }
      .modal { background: var(--paper); border: 1px solid var(--line); max-width: 380px; width: 100%; padding: 24px; }
      .modal__header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 6px; }
      .modal__header h3 { font-family: 'Noto Naskh Arabic', serif; font-size: 20px; }
      .modal__desc { font-size: 13.5px; color: var(--ink-muted); margin-bottom: 18px; }
      .modal__label { font-size: 12.5px; color: var(--ink-muted); margin: 10px 0 6px; display: block; }
      .modal__save { width: 100%; margin-top: 18px; padding: 11px; }

      .toast { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%); background: var(--ink); color: var(--paper); padding: 10px 22px; font-size: 13.5px; border-radius: 999px; z-index: 60; }

      @media (max-width: 720px) {
        .nav__inner { gap: 10px; }
        .nav__write span { display: none; }
        .nav__write { padding: 8px; }
        .nav__search { max-width: none; }
        .hero-card { grid-template-columns: 1fr; }
        .row-card { gap: 14px; }
        .row-card__thumb { width: 92px; height: 72px; }
        .article-title { font-size: 27px; }
        .article-content { font-size: 17px; }
      }
    `}</style>
  );
}
