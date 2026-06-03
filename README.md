# Datts
Search any book u want
import { useState, useEffect, useRef } from "react";

const styles = `
  @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400&family=DM+Sans:wght@300;400;500&display=swap');

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: #0a0a0f;
    font-family: 'DM Sans', sans-serif;
  }

  .app {
    min-height: 100vh;
    background: #0a0a0f;
    color: #f0ede6;
    position: relative;
    overflow-x: hidden;
  }

  .bg-orb {
    position: fixed;
    border-radius: 50%;
    filter: blur(120px);
    opacity: 0.15;
    pointer-events: none;
    z-index: 0;
  }
  .orb1 { width: 600px; height: 600px; background: #c8a96e; top: -200px; left: -200px; }
  .orb2 { width: 400px; height: 400px; background: #6e8fc8; bottom: 100px; right: -100px; }

  .header {
    position: relative;
    z-index: 1;
    padding: 60px 40px 40px;
    text-align: center;
    border-bottom: 1px solid rgba(200,169,110,0.15);
  }

  .logo-line {
    font-size: 11px;
    letter-spacing: 6px;
    text-transform: uppercase;
    color: #c8a96e;
    margin-bottom: 16px;
    font-weight: 300;
  }

  .title {
    font-family: 'Playfair Display', serif;
    font-size: clamp(36px, 6vw, 72px);
    font-weight: 900;
    line-height: 1;
    color: #f0ede6;
    margin-bottom: 12px;
  }

  .title span { color: #c8a96e; font-style: italic; }

  .subtitle {
    font-size: 15px;
    color: rgba(240,237,230,0.45);
    font-weight: 300;
    letter-spacing: 0.5px;
  }

  .search-section {
    position: relative;
    z-index: 1;
    padding: 50px 40px 30px;
    max-width: 800px;
    margin: 0 auto;
  }

  .search-wrap {
    display: flex;
    gap: 12px;
    align-items: stretch;
  }

  .search-input {
    flex: 1;
    background: rgba(240,237,230,0.05);
    border: 1px solid rgba(200,169,110,0.3);
    border-radius: 4px;
    padding: 18px 24px;
    font-family: 'DM Sans', sans-serif;
    font-size: 16px;
    color: #f0ede6;
    outline: none;
    transition: all 0.3s;
    letter-spacing: 0.3px;
  }

  .search-input::placeholder { color: rgba(240,237,230,0.3); }
  .search-input:focus {
    border-color: #c8a96e;
    background: rgba(200,169,110,0.07);
    box-shadow: 0 0 0 3px rgba(200,169,110,0.1);
  }

  .search-btn {
    background: #c8a96e;
    color: #0a0a0f;
    border: none;
    border-radius: 4px;
    padding: 18px 32px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    letter-spacing: 2px;
    text-transform: uppercase;
    cursor: pointer;
    transition: all 0.2s;
    white-space: nowrap;
  }

  .search-btn:hover { background: #dfc080; transform: translateY(-1px); }
  .search-btn:active { transform: translateY(0); }
  .search-btn:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

  .filters {
    display: flex;
    gap: 8px;
    margin-top: 16px;
    flex-wrap: wrap;
  }

  .filter-btn {
    background: transparent;
    border: 1px solid rgba(200,169,110,0.2);
    color: rgba(240,237,230,0.5);
    padding: 6px 16px;
    border-radius: 100px;
    font-family: 'DM Sans', sans-serif;
    font-size: 12px;
    letter-spacing: 1px;
    text-transform: uppercase;
    cursor: pointer;
    transition: all 0.2s;
  }

  .filter-btn:hover, .filter-btn.active {
    border-color: #c8a96e;
    color: #c8a96e;
    background: rgba(200,169,110,0.08);
  }

  .results-section {
    position: relative;
    z-index: 1;
    padding: 10px 40px 60px;
    max-width: 1100px;
    margin: 0 auto;
  }

  .results-header {
    font-size: 12px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: rgba(240,237,230,0.35);
    margin-bottom: 24px;
    padding-bottom: 12px;
    border-bottom: 1px solid rgba(240,237,230,0.06);
  }

  .results-header span { color: #c8a96e; }

  .books-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
  }

  .book-card {
    background: rgba(240,237,230,0.03);
    border: 1px solid rgba(240,237,230,0.08);
    border-radius: 6px;
    overflow: hidden;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    flex-direction: column;
    animation: fadeUp 0.4s ease both;
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .book-card:hover {
    border-color: rgba(200,169,110,0.4);
    background: rgba(200,169,110,0.05);
    transform: translateY(-3px);
    box-shadow: 0 12px 40px rgba(0,0,0,0.4);
  }

  .book-cover {
    width: 100%;
    height: 200px;
    object-fit: cover;
    background: rgba(200,169,110,0.08);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .book-cover img { width: 100%; height: 100%; object-fit: cover; }

  .cover-placeholder {
    width: 100%;
    height: 200px;
    background: linear-gradient(135deg, rgba(200,169,110,0.1), rgba(110,143,200,0.1));
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Playfair Display', serif;
    font-size: 48px;
    color: rgba(200,169,110,0.3);
  }

  .book-info {
    padding: 20px;
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .book-title {
    font-family: 'Playfair Display', serif;
    font-size: 17px;
    font-weight: 700;
    color: #f0ede6;
    margin-bottom: 6px;
    line-height: 1.3;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .book-author {
    font-size: 13px;
    color: #c8a96e;
    margin-bottom: 10px;
    font-weight: 300;
  }

  .book-meta {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    margin-top: auto;
    padding-top: 12px;
    border-top: 1px solid rgba(240,237,230,0.06);
  }

  .tag {
    font-size: 10px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: rgba(240,237,230,0.35);
    background: rgba(240,237,230,0.04);
    border: 1px solid rgba(240,237,230,0.08);
    padding: 4px 10px;
    border-radius: 2px;
  }

  .free-tag {
    color: #7ec87e;
    border-color: rgba(126,200,126,0.3);
    background: rgba(126,200,126,0.05);
  }

  /* Modal */
  .modal-overlay {
    position: fixed;
    inset: 0;
    background: rgba(10,10,15,0.92);
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
    backdrop-filter: blur(8px);
    animation: fadeIn 0.2s ease;
  }

  @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

  .modal {
    background: #111118;
    border: 1px solid rgba(200,169,110,0.2);
    border-radius: 8px;
    width: 100%;
    max-width: 750px;
    max-height: 88vh;
    overflow-y: auto;
    animation: slideUp 0.3s ease;
  }

  @keyframes slideUp {
    from { transform: translateY(30px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
  }

  .modal-header {
    padding: 30px 30px 24px;
    border-bottom: 1px solid rgba(200,169,110,0.12);
    position: sticky;
    top: 0;
    background: #111118;
    z-index: 1;
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 16px;
  }

  .modal-title {
    font-family: 'Playfair Display', serif;
    font-size: 24px;
    font-weight: 700;
    color: #f0ede6;
    line-height: 1.3;
  }

  .modal-author { font-size: 14px; color: #c8a96e; margin-top: 4px; font-weight: 300; }

  .close-btn {
    background: rgba(240,237,230,0.06);
    border: 1px solid rgba(240,237,230,0.1);
    color: rgba(240,237,230,0.6);
    width: 36px;
    height: 36px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s;
    flex-shrink: 0;
  }

  .close-btn:hover { background: rgba(200,169,110,0.1); color: #c8a96e; border-color: #c8a96e; }

  .modal-body { padding: 28px 30px; }

  .modal-desc {
    font-size: 15px;
    line-height: 1.8;
    color: rgba(240,237,230,0.7);
    margin-bottom: 28px;
  }

  .modal-links { display: flex; flex-direction: column; gap: 10px; margin-bottom: 28px; }

  .read-btn {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    padding: 16px 22px;
    border-radius: 4px;
    text-decoration: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    transition: all 0.2s;
    border: 1px solid;
  }

  .read-btn.primary {
    background: #c8a96e;
    color: #0a0a0f;
    border-color: #c8a96e;
  }
  .read-btn.primary:hover { background: #dfc080; }

  .read-btn.secondary {
    background: transparent;
    color: #c8a96e;
    border-color: rgba(200,169,110,0.3);
  }
  .read-btn.secondary:hover { background: rgba(200,169,110,0.08); border-color: #c8a96e; }

  .read-btn.tertiary {
    background: transparent;
    color: rgba(240,237,230,0.5);
    border-color: rgba(240,237,230,0.1);
    font-size: 13px;
  }
  .read-btn.tertiary:hover { color: #f0ede6; border-color: rgba(240,237,230,0.3); }

  .btn-label { flex: 1; }
  .btn-source { font-size: 11px; opacity: 0.6; letter-spacing: 1px; text-transform: uppercase; }
  .btn-arrow { font-size: 16px; }

  .modal-details {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
  }

  .detail-item {
    background: rgba(240,237,230,0.03);
    border: 1px solid rgba(240,237,230,0.06);
    border-radius: 4px;
    padding: 14px 16px;
  }

  .detail-label { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: rgba(240,237,230,0.3); margin-bottom: 4px; }
  .detail-value { font-size: 14px; color: #f0ede6; }

  .loading {
    text-align: center;
    padding: 60px 20px;
    color: rgba(240,237,230,0.4);
  }

  .spinner {
    width: 36px;
    height: 36px;
    border: 2px solid rgba(200,169,110,0.2);
    border-top-color: #c8a96e;
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    margin: 0 auto 16px;
  }

  @keyframes spin { to { transform: rotate(360deg); } }

  .empty {
    text-align: center;
    padding: 80px 20px;
    color: rgba(240,237,230,0.25);
  }

  .empty-icon { font-size: 52px; margin-bottom: 16px; }
  .empty-title { font-family: 'Playfair Display', serif; font-size: 22px; color: rgba(240,237,230,0.4); margin-bottom: 8px; }
  .empty-sub { font-size: 14px; }

  .error-msg {
    background: rgba(200,80,80,0.08);
    border: 1px solid rgba(200,80,80,0.2);
    color: rgba(240,150,150,0.8);
    padding: 14px 20px;
    border-radius: 4px;
    font-size: 14px;
    margin-bottom: 20px;
  }

  @media (max-width: 600px) {
    .header { padding: 40px 20px 30px; }
    .search-section { padding: 30px 20px 20px; }
    .results-section { padding: 10px 20px 40px; }
    .search-wrap { flex-direction: column; }
    .modal-header { padding: 20px; }
    .modal-body { padding: 20px; }
    .modal-details { grid-template-columns: 1fr; }
  }
`;

const SUBJECTS = ["All", "Physics", "Mathematics", "Chemistry", "Biology", "History", "Literature", "Computer Science"];

export default function BookFinder() {
  const [query, setQuery] = useState("");
  const [books, setBooks] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [selectedBook, setSelectedBook] = useState(null);
  const [searched, setSearched] = useState(false);
  const [activeSubject, setActiveSubject] = useState("All");
  const inputRef = useRef();

  const searchBooks = async (searchQuery = query) => {
    if (!searchQuery.trim()) return;
    setLoading(true);
    setError("");
    setBooks([]);
    setSearched(true);

    try {
      const q = encodeURIComponent(searchQuery.trim());
      const res = await fetch(`https://openlibrary.org/search.json?q=${q}&limit=24&fields=key,title,author_name,first_publish_year,subject,cover_i,number_of_pages_median,language,ia,has_fulltext`);
      const data = await res.json();

      const results = (data.docs || []).map(book => ({
        key: book.key,
        title: book.title,
        authors: book.author_name || ["Unknown Author"],
        year: book.first_publish_year,
        subjects: book.subject ? book.subject.slice(0, 4) : [],
        coverId: book.cover_i,
        pages: book.number_of_pages_median,
        languages: book.language || [],
        internetArchiveId: book.ia ? book.ia[0] : null,
        hasFulltext: book.has_fulltext,
      }));

      setBooks(results);
    } catch (e) {
      setError("Could not fetch books. Please check your connection and try again.");
    } finally {
      setLoading(false);
    }
  };

  const handleSubject = (subject) => {
    setActiveSubject(subject);
    if (subject !== "All") {
      setQuery(subject);
      searchBooks(subject);
    }
  };

  const openBook = async (book) => {
    setSelectedBook({ ...book, loading: true, description: "", links: [] });

    try {
      const workId = book.key.replace("/works/", "");
      const res = await fetch(`https://openlibrary.org${book.key}.json`);
      const data = await res.json();

      let desc = "";
      if (data.description) {
        desc = typeof data.description === "string" ? data.description : data.description.value || "";
      }

      const links = [];

      if (book.internetArchiveId) {
        links.push({
          label: "Read Full Book",
          sublabel: "Internet Archive",
          url: `https://archive.org/details/${book.internetArchiveId}`,
          type: "primary"
        });
      }

      links.push({
        label: "View on Open Library",
        sublabel: "Open Library",
        url: `https://openlibrary.org${book.key}`,
        type: "secondary"
      });

      links.push({
        label: "Search on Project Gutenberg",
        sublabel: "Project Gutenberg (free classics)",
        url: `https://www.gutenberg.org/ebooks/search/?query=${encodeURIComponent(book.title)}`,
        type: "tertiary"
      });

      links.push({
        label: "Search on Standard Ebooks",
        sublabel: "Standard Ebooks (free, polished)",
        url: `https://standardebooks.org/ebooks?query=${encodeURIComponent(book.title)}`,
        type: "tertiary"
      });

      setSelectedBook(prev => ({ ...prev, loading: false, description: desc, links }));
    } catch {
      setSelectedBook(prev => ({ ...prev, loading: false, description: "No description available.", links: [
        { label: "View on Open Library", sublabel: "Open Library", url: `https://openlibrary.org${book.key}`, type: "secondary" }
      ]}));
    }
  };

  return (
    <>
      <style>{styles}</style>
      <div className="app">
        <div className="bg-orb orb1" />
        <div className="bg-orb orb2" />

        <header className="header">
          <div className="logo-line">Free Knowledge · Open Access · Worldwide</div>
          <h1 className="title">The Open <span>Library</span></h1>
          <p className="subtitle">Search millions of free books — textbooks, classics, science, history & more</p>
        </header>

        <div className="search-section">
          <div className="search-wrap">
            <input
              ref={inputRef}
              className="search-input"
              type="text"
              placeholder="Search by title, author, or subject…"
              value={query}
              onChange={e => setQuery(e.target.value)}
              onKeyDown={e => e.key === "Enter" && searchBooks()}
            />
            <button className="search-btn" onClick={() => searchBooks()} disabled={loading || !query.trim()}>
              {loading ? "..." : "Search"}
            </button>
          </div>
          <div className="filters">
            {SUBJECTS.map(s => (
              <button
                key={s}
                className={`filter-btn ${activeSubject === s ? "active" : ""}`}
                onClick={() => handleSubject(s)}
              >
                {s}
              </button>
            ))}
          </div>
        </div>

        <div className="results-section">
          {error && <div className="error-msg">⚠ {error}</div>}

          {loading && (
            <div className="loading">
              <div className="spinner" />
              <div>Searching the archive…</div>
            </div>
          )}

          {!loading && searched && books.length > 0 && (
            <>
              <div className="results-header">
                Found <span>{books.length}</span> results for "{query}"
              </div>
              <div className="books-grid">
                {books.map((book, i) => (
                  <div
                    key={book.key}
                    className="book-card"
                    style={{ animationDelay: `${i * 0.04}s` }}
                    onClick={() => openBook(book)}
                  >
                    {book.coverId ? (
                      <img
                        src={`https://covers.openlibrary.org/b/id/${book.coverId}-M.jpg`}
                        alt={book.title}
                        style={{ width: "100%", height: 200, objectFit: "cover" }}
                      />
                    ) : (
                      <div className="cover-placeholder">📖</div>
                    )}
                    <div className="book-info">
                      <div className="book-title">{book.title}</div>
                      <div className="book-author">{book.authors.slice(0, 2).join(", ")}</div>
                      <div className="book-meta">
                        {book.year && <span className="tag">{book.year}</span>}
                        {book.pages && <span className="tag">{book.pages}p</span>}
                        {book.hasFulltext && <span className="tag free-tag">✓ Free</span>}
                        {book.internetArchiveId && <span className="tag free-tag">📖 Readable</span>}
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </>
          )}

          {!loading && searched && books.length === 0 && !error && (
            <div className="empty">
              <div className="empty-icon">🔍</div>
              <div className="empty-title">No books found</div>
              <div className="empty-sub">Try a different title, author, or subject</div>
            </div>
          )}

          {!searched && !loading && (
            <div className="empty">
              <div className="empty-icon">📚</div>
              <div className="empty-title">Search for any book</div>
              <div className="empty-sub">Access millions of free textbooks, classics, and study materials</div>
            </div>
          )}
        </div>

        {selectedBook && (
          <div className="modal-overlay" onClick={e => e.target === e.currentTarget && setSelectedBook(null)}>
            <div className="modal">
              <div className="modal-header">
                <div>
                  <div className="modal-title">{selectedBook.title}</div>
                  <div className="modal-author">{selectedBook.authors?.join(", ")}</div>
                </div>
                <button className="close-btn" onClick={() => setSelectedBook(null)}>×</button>
              </div>
              <div className="modal-body">
                {selectedBook.loading ? (
                  <div className="loading" style={{ padding: "30px 0" }}>
                    <div className="spinner" />
                    <div>Loading book details…</div>
                  </div>
                ) : (
                  <>
                    {selectedBook.description ? (
                      <p className="modal-desc">{selectedBook.description.slice(0, 600)}{selectedBook.description.length > 600 ? "…" : ""}</p>
                    ) : (
                      <p className="modal-desc" style={{ fontStyle: "italic", opacity: 0.4 }}>No description available for this book.</p>
                    )}

                    <div className="modal-links">
                      {selectedBook.links?.map((link, i) => (
                        <a key={i} href={link.url} target="_blank" rel="noopener noreferrer" className={`read-btn ${link.type}`}>
                          <div>
                            <div className="btn-label">{link.label}</div>
                            <div className="btn-source">{link.sublabel}</div>
                          </div>
                          <span className="btn-arrow">→</span>
                        </a>
                      ))}
                    </div>

                    <div className="modal-details">
                      {selectedBook.year && (
                        <div className="detail-item">
                          <div className="detail-label">First Published</div>
                          <div className="detail-value">{selectedBook.year}</div>
                        </div>
                      )}
                      {selectedBook.pages && (
                        <div className="detail-item">
                          <div className="detail-label">Pages</div>
                          <div className="detail-value">{selectedBook.pages}</div>
                        </div>
                      )}
                      {selectedBook.languages?.length > 0 && (
                        <div className="detail-item">
                          <div className="detail-label">Languages</div>
                          <div className="detail-value">{selectedBook.languages.slice(0, 3).join(", ")}</div>
                        </div>
                      )}
                      {selectedBook.subjects?.length > 0 && (
                        <div className="detail-item">
                          <div className="detail-label">Subjects</div>
                          <div className="detail-value">{selectedBook.subjects.slice(0, 2).join(", ")}</div>
                        </div>
                      )}
                    </div>
                  </>
                )}
              </div>
            </div>
          </div>
        )}
      </div>
    </>
  );
}
