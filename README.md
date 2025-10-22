          <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE html>
<html b:version='2' class='v2' expr:dir='data:blog.languageDirection' expr:lang='data:blog.locale' xmlns='http://www.w3.org/1999/xhtml' xmlns:b='http://www.google.com/2005/gml/b' xmlns:data='http://www.google.com/2005/gml/data' xmlns:expr='http://www.google.com/2005/gml/expr'>
<b:if cond='data:blog'>
<head>

  <!-- NOTE: Header info moved to a safe text block to avoid XML comment restrictions -->
  <script type="text/plain">
  <![CDATA[
  ALEO'S TUBE — SEO MAX HEAD v10.5 (FULL READY)
  Features:
  - XML-safe for Blogger (CDATA where needed)
  - GA4, JSON-LD, Open Graph, Twitter Card
  - Auto Voice Welcome (multi-language incl. Tetun)
  - Auto Translate (auto detect visitor language, hides UI)
  - Backlink Tracker widget + Chart.js
  - Sync added backlinks to Google Sheets via Apps Script Web App
  ]]>
  </script>

  <!-- Basic -->
  <meta charset='UTF-8'/>
  <meta name='viewport' content='width=device-width, initial-scale=1'/>
  <meta http-equiv='X-UA-Compatible' content='IE=edge'/>

  <!-- Dynamic title (Blogger variable) -->
  <title><data:blog.pageTitle/></title>

  <!-- Canonical & robots -->
  <link rel='canonical' expr:href='data:blog.canonicalUrl'/>
  <meta name='robots' content='index,follow'/>

  <!-- Core SEO meta (dynamic) -->
  <meta expr:content='data:blog.metaDescription' name='description'/>
  <meta expr:content='data:blog.metaKeywords' name='keywords'/>
  <meta expr:content='data:blog.author' name='author'/>

  <!-- Open Graph -->
  <meta property='og:site_name' expr:content='data:blog.title'/>
  <meta property='og:title' expr:content='data:blog.pageTitle'/>
  <meta property='og:description' expr:content='data:blog.metaDescription'/>
  <meta property='og:url' expr:content='data:blog.canonicalUrl'/>
  <meta property='og:type' content='website'/>
  <meta property='og:image' expr:content='data:blog.postImageUrl'/>

  <!-- Twitter Card -->
  <meta name='twitter:card' content='summary_large_image'/>
  <meta name='twitter:title' expr:content='data:blog.pageTitle'/>
  <meta name='twitter:description' expr:content='data:blog.metaDescription'/>
  <meta name='twitter:image' expr:content='data:blog.postImageUrl'/>

  <!-- JSON-LD Structured Data (wrapped in CDATA to be XML-safe) -->
  <script type='application/ld+json'>
  <![CDATA[
  {
    "@context": "https://schema.org",
    "@graph": [
      {
        "@type": "Organization",
        "name": "<data:blog.title/>",
        "url": "<data:blog.homepageUrl/>",
        "logo": "<data:blog.logoUrl/>",
        "sameAs": []
      },
      {
        "@type": "WebSite",
        "url": "<data:blog.homepageUrl/>",
        "name": "<data:blog.title/>",
        "potentialAction": {
          "@type": "SearchAction",
          "target": "<data:blog.homepageUrl/>search?q={search_term_string}",
          "query-input": "required name=search_term_string"
        }
      }
    ]
  }
  ]]>
  </script>

  <!-- Google Analytics (GA4) - replace ID if needed -->
  <script async src='https://www.googletagmanager.com/gtag/js?id=G-71BR09BJ1R'></script>
  <script type='text/javascript'>
  <![CDATA[
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-71BR09BJ1R', { 'send_page_view': true });
  ]]>
  </script>

  <!-- Minimal styles for ALEO widget -->
  <style type='text/css'>
    #aleo-tools{font-family:system-ui,Arial;font-size:13px}
    #aleo-tools .card{background:rgba(255,255,255,0.96);border:1px solid rgba(0,0,0,0.06);padding:12px;border-radius:10px;box-shadow:0 6px 18px rgba(0,0,0,0.04)}
    #aleo-tools.compact{position:fixed;right:12px;bottom:12px;width:320px;z-index:99999}
    @media(max-width:720px){#aleo-tools.compact{display:none}}
    #aleo-tools h4{margin:0 0 8px;font-size:14px}
    #aleo-tools input,#aleo-tools button,#aleo-tools textarea{width:100%;margin:4px 0;padding:8px;border-radius:6px;border:1px solid #ddd;box-sizing:border-box}
    #aleo-seo-health{margin-top:8px;font-size:12px}
  </style>

  <!-- Chart.js CDN (deferred) -->
  <script src='https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js' defer></script>

  <!-- Backlink Tracker + Chart + Google Sheets sync
       NOTE: Replace SHEET_URL value with your Apps Script Web App URL (doPost).
  -->
  <script type='text/javascript'>
  <![CDATA[
  (function(){
    const LS_KEY = 'aleo_backlinks_v1';
    const SHEET_URL = 'GANTI_DENGAN_URL_WEB_APP'; // <-- REPLACE this with your deployed Apps Script Web App URL

    function loadLocal(){
      try{ return JSON.parse(localStorage.getItem(LS_KEY)) || {targets:[]}; }
      catch(e){ return {targets:[]}; }
    }
    function saveLocal(d){
      try{ localStorage.setItem(LS_KEY, JSON.stringify(d)); }catch(e){}
    }

    // Public API
    window.ALEO = {
      get: function(){ return loadLocal(); },
      add: async function(url, notes){
        const store = loadLocal();
        const entry = { url: String(url), notes: notes ? String(notes) : '', added: new Date().toISOString() };
        store.targets.push(entry);
        saveLocal(store);
        // Try send to Google Sheets via Apps Script Web App (best-effort; no-cors possible)
        try{
          await fetch(SHEET_URL, { method: 'POST', mode: 'no-cors', body: JSON.stringify(entry) });
        }catch(e){
          // fail silently — local copy remains
        }
        return store;
      }
    };

    // Simple SEO health checks
    function seoHealth(){
      const t = document.title || '';
      const md = document.querySelector('meta[name="description"]');
      const len = md ? (md.getAttribute('content') || '').length : 0;
      return [
        {k:'title', ok: (t.length>10 && t.length<130), v: t.length},
        {k:'description', ok: (len>50 && len<320), v: len},
        {k:'canonical', ok: !!document.querySelector('link[rel="canonical"]'), v: location.href}
      ];
    }

    // Render widget & chart
    function renderWidget(){
      if(document.getElementById('aleo-tools')) return;
      const d = document.createElement('div'); d.id='aleo-tools'; d.className='compact';
      d.innerHTML = '<div class="card"><h4>SEO & Backlink — ALEO</h4>'
        + '<input id="a-url" placeholder="Tambah target backlink (https://...)"/>'
        + '<input id="a-note" placeholder="Catatan (opsional)"/>'
        + '<button id="a-add">Tambah</button>'
        + '<div id="aleo-seo-health"><strong>SEO Health:</strong><div id="a-health"></div></div>'
        + '<canvas id="a-chart" height="80"></canvas>'
        + '<div style="font-size:11px;margin-top:6px">Data lokal + Google Sheets (optional).</div>'
        + '<button id="a-exp" style="margin-top:6px">Export CSV</button>'
        + '</div>';
      document.body.appendChild(d);

      function refreshHealth(){
        const list = seoHealth(); const box = document.getElementById('a-health'); box.innerHTML = '';
        list.forEach(it => { const el = document.createElement('div'); el.textContent = '• ' + it.k + ': ' + (it.ok ? 'OK' : 'Periksa') + ' (' + it.v + ')'; box.appendChild(el); });
      }
      refreshHealth();

      document.getElementById('a-add').addEventListener('click', async function(){
        const url = document.getElementById('a-url').value.trim();
        const note = document.getElementById('a-note').value.trim();
        if(!url){ alert('Masukkan URL target backlink.'); return; }
        await window.ALEO.add(url, note);
        document.getElementById('a-url').value=''; document.getElementById('a-note').value='';
        refreshHealth(); redrawChart();
      });

      document.getElementById('a-exp').addEventListener('click', function(){
        const s = window.ALEO.get(); const rows = ['url,added,notes'];
        s.targets.forEach(t => rows.push('"' + t.url + '","' + t.added + '","' + (t.notes||'') + '"'));
        const blob = new Blob([rows.join('\n')], {type:'text/csv'}); const u = URL.createObjectURL(blob);
        const a = document.createElement('a'); a.href = u; a.download = 'aleo-backlinks.csv'; a.click(); URL.revokeObjectURL(u);
      });

      // Chart draw
      function drawChart(){
        try{
          const s = window.ALEO.get(); const counts = {};
          s.targets.forEach(t => { const m = (new Date(t.added)).toISOString().slice(0,7); counts[m] = (counts[m]||0)+1; });
          const labels = Object.keys(counts).sort(); const data = labels.map(l=>counts[l]);
          const ctx = document.getElementById('a-chart').getContext('2d');
          if(window.ALEO._chart){ try{ window.ALEO._chart.destroy(); }catch(e){} }
          window.ALEO._chart = new Chart(ctx, {
            type: 'line',
            data: { labels: labels, datasets: [{ label: 'Backlinks', data: data, fill: true, tension: 0.3 }] },
            options:{ plugins:{ legend:{ display:false } }, scales:{ y:{ beginAtZero:true } } }
          });
        }catch(e){}
      }

      function redrawChart(){ drawChart(); }
      drawChart();
      window.ALEO.redrawChart = redrawChart;
    }

    if(document.readyState === 'loading') document.addEventListener('DOMContentLoaded', renderWidget); else renderWidget();

  })();
  ]]>
  </script>

  <!-- Voice Welcome (multi-language incl. Tetun 'tet') -->
  <script type='text/javascript'>
  <![CDATA[
  (function(){
    const phrases = {
      en: 'Hello friend - welcome! I can read this page for you. (Aleo)',
      id: 'Hai sobat - selamat datang! Aku bisa membacakan halaman ini. (Aleo)',
      ms: 'Hai kawan - selamat datang! Saya boleh membaca halaman ini untuk anda. (Aleo)',
      es: 'Hola amigo - bienvenido! Puedo leer la página para ti. (Aleo)',
      fr: 'Bonjour - bienvenue! Je peux lire la page pour vous. (Aleo)',
      tet: 'Ola - diak ba! Bem-vinda! (Aleo)'
    };
    function speak(msg){
      try{
        const u = new SpeechSynthesisUtterance(msg);
        u.lang = (navigator.language || 'en-US');
        u.volume = 0.9; u.rate = 0.95; u.pitch = 1;
        window.speechSynthesis.cancel(); window.speechSynthesis.speak(u);
      }catch(e){}
    }
    function choose(){
      const code = (navigator.language || 'en').slice(0,2);
      return phrases[code] || phrases['en'];
    }
    let fired=false;
    function init(){ if(!fired){ speak(choose()); fired = true; } }
    document.addEventListener('DOMContentLoaded', init);
    ['click','touchstart','keydown'].forEach(evt => window.addEventListener(evt, init, { once:true }));
  })();
  ]]>
  </script>

  <!-- Auto Translate ALL languages (auto-detect visitor language) -->
  <script type='text/javascript'>
  <![CDATA[
  (function(){
    if(location.pathname.indexOf('/admin') > -1) return;
    window.addEventListener('load', function(){
      try{
        var s = document.createElement('script');
        s.src = '//translate.google.com/translate_a/element.js?cb=__gtInit';
        s.type = 'text/javascript';
        s.async = true;
        document.head.appendChild(s);
      }catch(e){}
    });

    window.__gtInit = function(){
      try{
        new google.translate.TranslateElement({ pageLanguage: 'id', autoDisplay: false, layout: google.translate.TranslateElement.InlineLayout.SIMPLE }, 'google_translate_element');
        var lang = (navigator.language || 'id').slice(0,2);
        document.cookie = 'googtrans=/id/' + lang + '; path=/;';
        setTimeout(function(){
          try{
            var el = document.querySelector('.goog-te-combo');
            if(el){ el.value = lang; el.dispatchEvent(new Event('change')); }
          }catch(e){}
        }, 700);
      }catch(e){}
    };

    var d = document.createElement('div'); d.id = 'google_translate_element'; d.style.display = 'none';
    document.addEventListener('DOMContentLoaded', function(){ document.body.appendChild(d); });
  })();
  ]]>
  </script>

  <!-- END head -->
</head>
</b:if>
 
/* Header
----------------------------------------------- */
.header-outer {
  background: $(header.background.color) $(header.background.gradient) repeat-x scroll top left;
  _background-image: none;

  color: $(header.text.color);

  -moz-border-radius: $(header.border.radius);
  -webkit-border-radius: $(header.border.radius);
  -goog-ms-border-radius: $(header.border.radius);
  border-radius: $(header.border.radius);
}

.Header img, .Header #header-inner {
  -moz-border-radius: $(header.border.radius);
  -webkit-border-radius: $(header.border.radius);
  -goog-ms-border-radius: $(header.border.radius);
  border-radius: $(header.border.radius);
}

.header-inner .Header .titlewrapper,
.header-inner .Header .descriptionwrapper {
  padding-left: $(header.padding);
  padding-right: $(header.padding);
}

.Header h1 {
  font: $(header.font);
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.3);
}

.Header h1 a {
  color: $(header.text.color);
}

.Header .description {
  font-size: 130%;
}

/* Tabs
----------------------------------------------- */
.tabs-inner {
  margin: .5em $(tabs.margin.sides) $(tabs.margin.bottom);
  padding: 0;
}

.tabs-inner .section {
  margin: 0;
}

.tabs-inner .widget ul {
  padding: 0;

  background: $(tabs.background.color) $(tabs.background.gradient) repeat scroll bottom;

  -moz-border-radius: $(tabs.border.radius);
  -webkit-border-radius: $(tabs.border.radius);
  -goog-ms-border-radius: $(tabs.border.radius);
  border-radius: $(tabs.border.radius);
}

.tabs-inner .widget li {
  border: none;
}

.tabs-inner .widget li a {
  display: inline-block;

  padding: .5em 1em;
  margin-$endSide: $(tabs.spacing);

  color: $(tabs.text.color);
  font: $(tabs.font);

  -moz-border-radius: $(tab.border.radius) $(tab.border.radius) 0 0;
  -webkit-border-top-left-radius: $(tab.border.radius);
  -webkit-border-top-right-radius: $(tab.border.radius);
  -goog-ms-border-radius: $(tab.border.radius) $(tab.border.radius) 0 0;
  border-radius: $(tab.border.radius) $(tab.border.radius) 0 0;

  background: $(tab.background);

  border-$endSide: 1px solid $(tabs.separator.color);
}

.tabs-inner .widget li:first-child a {
  padding-$startSide: 1.25em;

  -moz-border-radius-top$startSide: $(tab.first.border.radius);
  -moz-border-radius-bottom$startSide: $(tabs.border.radius);
  -webkit-border-top-$startSide-radius: $(tab.first.border.radius);
  -webkit-border-bottom-$startSide-radius: $(tabs.border.radius);
  -goog-ms-border-top-$startSide-radius: $(tab.first.border.radius);
  -goog-ms-border-bottom-$startSide-radius: $(tabs.border.radius);
  border-top-$startSide-radius: $(tab.first.border.radius);
  border-bottom-$startSide-radius: $(tabs.border.radius);
}

.tabs-inner .widget li.selected a,
.tabs-inner .widget li a:hover {
  position: relative;
  z-index: 1;

  background: $(tabs.selected.background.color) $(tab.selected.background.gradient) repeat scroll bottom;
  color: $(tabs.selected.text.color);

  -moz-box-shadow: 0 0 $(region.shadow.spread) rgba(0, 0, 0, .15);
  -webkit-box-shadow: 0 0 $(region.shadow.spread) rgba(0, 0, 0, .15);
  -goog-ms-box-shadow: 0 0 $(region.shadow.spread) rgba(0, 0, 0, .15);
  box-shadow: 0 0 $(region.shadow.spread) rgba(0, 0, 0, .15);
}

/* Headings
----------------------------------------------- */
h2 {
  font: $(widget.title.font);
  text-transform: $(widget.title.text.transform);
  color: $(widget.title.text.color);
  margin: .5em 0;
}

/* Main
----------------------------------------------- */
.main-outer {
  background: $(main.background);

  -moz-border-radius: $(main.border.radius.top) $(main.border.radius.top) 0 0;
  -webkit-border-top-left-radius: $(main.border.radius.top);
  -webkit-border-top-right-radius: $(main.border.radius.top);
  -webkit-border-bottom-left-radius: 0;
  -webkit-border-bottom-right-radius: 0;
  -goog-ms-border-radius: $(main.border.radius.top) $(main.border.radius.top) 0 0;
  border-radius: $(main.border.radius.top) $(main.border.radius.top) 0 0;

  -moz-box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
  -webkit-box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
  -goog-ms-box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
  box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
}

.main-inner {
  padding: 15px $(main.padding.sides) 20px;
}

.main-inner .column-center-inner {
  padding: 0 0;
}

.main-inner .column-left-inner {
  padding-left: 0;
}

.main-inner .column-right-inner {
  padding-right: 0;
}

/* Posts
----------------------------------------------- */
h3.post-title {
  margin: 0;
  font: $(post.title.font);
}

.comments h4 {
  margin: 1em 0 0;
  font: $(post.title.font);
}

.date-header span {
  color: $(date.header.color);
}

.post-outer {
  background-color: $(post.background.color);
  border: solid 1px $(post.border.color);

  -moz-border-radius: $(post.border.radius);
  -webkit-border-radius: $(post.border.radius);
  border-radius: $(post.border.radius);
  -goog-ms-border-radius: $(post.border.radius);

  padding: 15px 20px;
  margin: 0 $(post.margin.sides) 20px;
}

.post-body {
  line-height: 1.4;
  font-size: 110%;
  position: relative;
}

.post-header {
  margin: 0 0 1.5em;

  color: $(post.footer.text.color);
  line-height: 1.6;
}

.post-footer {
  margin: .5em 0 0;

  color: $(post.footer.text.color);
  line-height: 1.6;
}

#blog-pager {
  font-size: 140%
}

#comments .comment-author {
  padding-top: 1.5em;

  border-top: dashed 1px #ccc;
  border-top: dashed 1px rgba(128, 128, 128, .5);

  background-position: 0 1.5em;
}

#comments .comment-author:first-child {
  padding-top: 0;

  border-top: none;
}

.avatar-image-container {
  margin: .2em 0 0;
}

/* Comments
----------------------------------------------- */
.comments .comments-content .icon.blog-author {
  background-repeat: no-repeat;
  background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAASCAYAAABWzo5XAAAAAXNSR0IArs4c6QAAAAZiS0dEAP8A/wD/oL2nkwAAAAlwSFlzAAALEgAACxIB0t1+/AAAAAd0SU1FB9sLFwMeCjjhcOMAAAD+SURBVDjLtZSvTgNBEIe/WRRnm3U8RC1neQdsm1zSBIU9VVF1FkUguQQsD9ITmD7ECZIJSE4OZo9stoVjC/zc7ky+zH9hXwVwDpTAWWLrgS3QAe8AZgaAJI5zYAmc8r0G4AHYHQKVwII8PZrZFsBFkeRCABYiMh9BRUhnSkPTNCtVXYXURi1FpBDgArj8QU1eVXUzfnjv7yP7kwu1mYrkWlU33vs1QNu2qU8pwN0UpKoqokjWwCztrMuBhEhmh8bD5UDqur75asbcX0BGUB9/HAMB+r32hznJgXy2v0sGLBcyAJ1EK3LFcbo1s91JeLwAbwGYu7TP/3ZGfnXYPgAVNngtqatUNgAAAABJRU5ErkJggg==);
}

.comments .comments-content .loadmore a {
  border-top: 1px solid $(link.hover.color);
  border-bottom: 1px solid $(link.hover.color);
}

.comments .continue {
  border-top: 2px solid $(link.hover.color);
}

/* Widgets
----------------------------------------------- */
.widget ul, .widget #ArchiveList ul.flat {
  padding: 0;
  list-style: none;
}

.widget ul li, .widget #ArchiveList ul.flat li {
  border-top: dashed 1px #ccc;
  border-top: dashed 1px rgba(128, 128, 128, .5);
}

.widget ul li:first-child, .widget #ArchiveList ul.flat li:first-child {
  border-top: none;
}

.widget .post-body ul {
  list-style: disc;
}

.widget .post-body ul li {
  border: none;
}

/* Footer
----------------------------------------------- */
.footer-outer {
  color:$(footer.text.color);
  background: $(footer.background);

  -moz-border-radius: $(footer.border.radius.top) $(footer.border.radius.top) $(footer.border.radius.bottom) $(footer.border.radius.bottom);
  -webkit-border-top-left-radius: $(footer.border.radius.top);
  -webkit-border-top-right-radius: $(footer.border.radius.top);
  -webkit-border-bottom-left-radius: $(footer.border.radius.bottom);
  -webkit-border-bottom-right-radius: $(footer.border.radius.bottom);
  -goog-ms-border-radius: $(footer.border.radius.top) $(footer.border.radius.top) $(footer.border.radius.bottom) $(footer.border.radius.bottom);
  border-radius: $(footer.border.radius.top) $(footer.border.radius.top) $(footer.border.radius.bottom) $(footer.border.radius.bottom);

  -moz-box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
  -webkit-box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
  -goog-ms-box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
  box-shadow: 0 $(region.shadow.offset) $(region.shadow.spread) rgba(0, 0, 0, .15);
}

.footer-inner {
  padding: 10px $(main.padding.sides) 20px;
}

.footer-outer a {
  color: $(footer.link.color);
}

.footer-outer a:visited {
  color: $(footer.link.visited.color);
}

.footer-outer a:hover {
  color: $(footer.link.hover.color);
}

.footer-outer .widget h2 {
  color: $(footer.widget.title.text.color);
}

/* Mobile
----------------------------------------------- */
html body.mobile {
  height: auto;
}

html body.mobile {
  min-height: 480px;
  background-size: 100% auto;
}

.mobile .body-fauxcolumn-outer {
  background: $(mobile.background.overlay);
}

html .mobile .mobile-date-outer, html .mobile .blog-pager {
  border-bottom: none;
  background: $(main.background);
  margin-bottom: 10px;
}

.mobile .date-outer {
  background: $(main.background);
}

.mobile .header-outer, .mobile .main-outer,
.mobile .post-outer, .mobile .footer-outer {
  -moz-border-radius: 0;
  -webkit-border-radius: 0;
  -goog-ms-border-radius: 0;
  border-radius: 0;
}

.mobile .content-outer,
.mobile .main-outer,
.mobile .post-outer {
  background: inherit;
  border: none;
}

.mobile .content-outer {
  font-size: 100%;
}

.mobile-link-button {
  background-color: $(link.color);
}

.mobile-link-button a:link, .mobile-link-button a:visited {
  color: $(post.background.color);
}

.mobile-index-contents {
  color: $(body.text.color);
}

.mobile .tabs-inner .PageList .widget-content {
  background: $(tabs.selected.background.color) $(tab.selected.background.gradient) repeat scroll bottom;
  color: $(tabs.selected.text.color);
}

.mobile .tabs-inner .PageList .widget-content .pagelist-arrow {
  border-$startSide: 1px solid $(tabs.separator.color);
}
 ]]></b:skin>

    <b:template-skin>
      <b:variable default='960px' name='content.width' type='length' value='700px'/>
      <b:variable default='100%' name='main.column.left.width' type='length' value='0px'/>
      <b:variable default='310px' name='main.column.right.width' type='length' value='0px'/>

      <![CDATA[
      body {
        min-width: $(content.width);
      }

      .content-outer, .content-fauxcolumn-outer, .region-inner {
        min-width: $(content.width);
        max-width: $(content.width);
        _width: $(content.width);
      }

      .main-inner .columns {
        padding-left: $(main.column.left.width);
        padding-right: $(main.column.right.width);
      }

      .main-inner .fauxcolumn-center-outer {
        left: $(main.column.left.width);
        right: $(main.column.right.width);
        /* IE6 does not respect left and right together */
        _width: expression(this.parentNode.offsetWidth -
            parseInt("$(main.column.left.width)") -
            parseInt("$(main.column.right.width)") + 'px');
      }

      .main-inner .fauxcolumn-left-outer {
        width: $(main.column.left.width);
      }

      .main-inner .fauxcolumn-right-outer {
        width: $(main.column.right.width);
      }

      .main-inner .column-left-outer {
        width: $(main.column.left.width);
        right: 100%;
        margin-left: -$(main.column.left.width);
      }

      .main-inner .column-right-outer {
        width: $(main.column.right.width);
        margin-right: -$(main.column.right.width);
      }

      #layout {
        min-width: 0;
      }

      #layout .content-outer {
        min-width: 0;
        width: 800px;
      }

      #layout .region-inner {
        min-width: 0;
        width: auto;
      }

      body#layout div.add_widget {
        padding: 8px;
      }

      body#layout div.add_widget a {
        margin-left: 32px;
      }
      ]]>
    </b:template-skin>

    <b:if cond='data:skin.vars.body_background.image.isResizable'>
      <b:include cond='not data:view.isPreview' data='{                          image: data:skin.vars.body_background.image,                          selector: &quot;body&quot;                        }' name='responsiveImageStyle'/>
    </b:if>

    <b:include data='blog' name='google-analytics'/>
  </head>
<script type='text/javascript'>
//<![CDATA[
function updateClockFooter() {
  const now = new Date();
  const second = now.getSeconds() * 6;
  const minute = now.getMinutes() * 6 + second / 60;
  const hour = ((now.getHours() % 12) / 12) * 360 + minute / 12;

  document.querySelector('.analog-clock-footer .hand.hour').style.transform =
    `rotate(${hour}deg) translate(-50%, -50%)`;
  document.querySelector('.analog-clock-footer .hand.minute').style.transform =
    `rotate(${minute}deg) translate(-50%, -50%)`;
  document.querySelector('.analog-clock-footer .hand.second').style.transform =
    `rotate(${second}deg) translate(-50%, -50%)`;
}

setInterval(updateClockFooter, 1000);
updateClockFooter();
//]]>
</script>

  <!-- Hybrid Footer Super Ramping Mungil + Tombol Subscribe Pelangi Floating -->
<link href='https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css' rel='stylesheet'/>
<style>
  :root {
    --primary-color: #ffcc00;
    --subscribe-color: #ff0000;
    --light-bg: rgba(255,255,255,0.8);
    --light-text: #222;
    --dark-bg: rgba(20,20,20,0.8);
    --dark-text: #fff;
  }

  /* Footer mungil lonjong */
  footer.mini-footer {
    position: fixed;
    bottom: 58px;
    left: 50%;
    transform: translateX(-50%);
    background: var(--light-bg);
    color: var(--light-text);
    border-radius: 20px;
    padding: 3px 10px;
    font-size: 10px;
    font-family: Arial, sans-serif;
    box-shadow: 0 2px 6px rgba(0,0,0,0.15);
    z-index: 900;
    opacity: 0;
    animation: fadeUp 0.6s ease forwards;
  }
  @media (prefers-color-scheme: dark) {
    footer.mini-footer { background: var(--dark-bg); color: var(--dark-text); }
  }

  /* Navbar mungil */
  .mini-nav {
    position: fixed;
    bottom: 8px;
    left: 50%;
    transform: translateX(-50%);
    background: var(--light-bg);
    border-radius: 30px;
    display: flex;
    gap: 12px;
    padding: 4px 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.15);
    z-index: 1000;
    opacity: 0;
    animation: fadeUp 0.8s ease forwards;
  }
  .mini-nav a {
    color: var(--light-text);
    font-size: 11px;
    text-decoration: none;
    display: flex;
    align-items: center;
    transition: all 0.2s ease;
  }
  .mini-nav a i { font-size: 13px; margin-right: 3px; }
  .mini-nav a:hover { color: var(--primary-color); }
  @media (prefers-color-scheme: dark) {
    .mini-nav { background: var(--dark-bg); }
    .mini-nav a { color: var(--dark-text); }
  }

  /* Tombol Subscribe mungil dengan border pelangi + glow + floating */
  .mini-subscribe {
    position: fixed;
    right: 12px;
    bottom: 100px;
    background: var(--subscribe-color);
    color: #fff;
    font-size: 11px;
    padding: 5px 12px;
    border-radius: 30px;
    text-decoration: none;
    font-weight: bold;
    z-index: 1100;
    display: inline-block !important;
    opacity: 0;
    animation: fadeUp 1s ease forwards, glowPulse 2s infinite, floatBounce 3s ease-in-out infinite;
    border: 2px solid transparent;
    background-clip: padding-box;
    position: relative;
  }

  /* Border pelangi */
  .mini-subscribe::before {
    content: &quot;&quot;;
    position: absolute;
    inset: -2px;
    border-radius: 30px;
    padding: 2px;
    background: linear-gradient(90deg, red, orange, yellow, green, blue, indigo, violet, red);
    background-size: 400%;
    -webkit-mask: 
      linear-gradient(#fff 0 0) content-box, 
      linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
            mask-composite: exclude;
    animation: rainbow 4s linear infinite;
  }

  .mini-subscribe:hover {
    background: #cc0000;
    transform: scale(1.1);
  }

  /* Animasi border pelangi */
  @keyframes rainbow {
    0% { background-position: 0% 50%; }
    100% { background-position: 400% 50%; }
  }

  /* Efek glow kedip */
  @keyframes glowPulse {
    0%, 100% { box-shadow: 0 0 6px rgba(255,0,0,0.6); }
    50% { box-shadow: 0 0 14px rgba(255,0,0,0.9); }
  }

  /* Animasi melayang naik-turun */
  @keyframes floatBounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-8px); }
  }

  /* Animasi muncul */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(15px) translateX(-50%); }
    to { opacity: 1; transform: translateY(0) translateX(-50%); }
  }
</style>

<!-- Navbar mungil -->
<div class='mini-nav'>
  <a href='/'><i class='fas fa-home'/>Home</a>
  <a href='/p/about.html'><i class='fas fa-user'/>About</a>
  <a href='https://www.youtube.com/@Daily_vlog_anak_rantau' target='_blank'><i class='fab fa-youtube'/>YT</a>
  <a href='/p/contact.html'><i class='fas fa-envelope'/>Kontak</a>
</div>

<!-- Google Tag Manager (noscript) -->
<noscript>
  <iframe height='0' src='https://www.googletagmanager.com/ns.html?id=GTM-T5PTHLRB' style='display:none;visibility:hidden' width='0'/>
  </noscript>
  <body>
    <!-- Google Tag Manager (noscript) -->
<noscript>
  <iframe height='0' src='https://www.googletagmanager.com/ns.html?id=GTM-MKB9TPDR' style='display:none;visibility:hidden' width='0'/>
</noscript>
<!-- End Google Tag Manager (noscript) -->
    <!-- Google Tag Manager (noscript) -->
<noscript>
  <iframe height='0' src='https://www.googletagmanager.com/ns.html?id=GTM-WR546M4D&amp;gtm_auth=7_SAhutJuvX_5wv88hZpYg&amp;gtm_preview=env-10&amp;gtm_cookies_win=x' style='display:none;visibility:hidden' width='0'/>
</noscript>
<!-- End Google Tag Manager (noscript) -->
</body>
  <body expr:class='&quot;loading&quot; + data:blog.mobileClass'>
  <b:section class='navbar' id='navbar' maxwidgets='1' name='Navbar' showaddelement='no'>
    <b:widget id='Navbar1' locked='false' title='Navbar' type='Navbar'>
      <b:includable id='main'>&lt;script type=&quot;text/javascript&quot;&gt;
    function setAttributeOnload(object, attribute, val) {
      if(window.addEventListener) {
        window.addEventListener(&#39;load&#39;,
          function(){ object[attribute] = val; }, false);
      } else {
        window.attachEvent(&#39;onload&#39;, function(){ object[attribute] = val; });
      }
    }
  &lt;/script&gt;
&lt;div id=&quot;navbar-iframe-container&quot;&gt;&lt;/div&gt;
&lt;script type=&quot;text/javascript&quot; src=&quot;https://apis.google.com/js/platform.js&quot;&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot;&gt;
      gapi.load(&quot;gapi.iframes:gapi.iframes.style.bubble&quot;, function() {
        if (gapi.iframes &amp;&amp; gapi.iframes.getContext) {
          gapi.iframes.getContext().openChild({
              url: &#39;https://draft.blogger.com/navbar/8956348688248328686?origin\x3dhttp://localhost:80&#39;,
              where: document.getElementById(&quot;navbar-iframe-container&quot;),
              id: &quot;navbar-iframe&quot;
          });
        }
      });
    &lt;/script&gt;&lt;script type=&quot;text/javascript&quot;&gt;
(function() {
var script = document.createElement(&#39;script&#39;);
script.type = &#39;text/javascript&#39;;
script.src = &#39;//pagead2.googlesyndication.com/pagead/js/google_top_exp.js&#39;;
var head = document.getElementsByTagName(&#39;head&#39;)[0];
if (head) {
head.appendChild(script);
}})();
&lt;/script&gt;
</b:includable>
    </b:widget>
  </b:section>

  <b:if cond='data:blog.pageType == &quot;index&quot;'>
    <div itemscope='itemscope' itemtype='http://schema.org/Blog' style='display: none;'>
      <meta expr:content='data:blog.title' itemprop='name'/>
      <b:if cond='data:blog.metaDescription'>
        <meta expr:content='data:blog.metaDescription' itemprop='description'/>
      </b:if>
    </div>
  </b:if>

  <div class='body-fauxcolumns'>
    <div class='fauxcolumn-outer body-fauxcolumn-outer'>
    <div class='cap-top'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    <div class='fauxborder-left'>
    <div class='fauxborder-right'/>
    <div class='fauxcolumn-inner'>
    </div>
    </div>
    <div class='cap-bottom'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    </div>
  </div>

  <div class='content'>
  <div class='content-fauxcolumns'>
    <div class='fauxcolumn-outer content-fauxcolumn-outer'>
    <div class='cap-top'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    <div class='fauxborder-left'>
    <div class='fauxborder-right'/>
    <div class='fauxcolumn-inner'>
    </div>
    </div>
    <div class='cap-bottom'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    </div>
  </div>

  <div class='content-outer'>
  <div class='content-cap-top cap-top'>
    <div class='cap-left'/>
    <div class='cap-right'/>
  </div>
  <div class='fauxborder-left content-fauxborder-left'>
  <div class='fauxborder-right content-fauxborder-right'/>
  <div class='content-inner'>
    <header><!-- CSS + Icons -->
<link href='https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css' rel='stylesheet'/>
<style>
  .linktree-mini {
    text-align: center;
    margin: 15px auto;
    background: transparent;
  }

  /* Avatar efek api */
  .avatar-fire {
    position: relative;
    display: inline-block;
    margin-bottom: 20px;
  }
  .avatar-fire img {
    width: 90px;
    height: 90px;
    border-radius: 50%;
    object-fit: cover;
    z-index: 3;
    position: relative;
  }

  /* Lidah api */
  .flame {
    position: absolute;
    top: -25px; left: -25px; right: -25px; bottom: -25px;
    border-radius: 50%;
    filter: blur(28px);
    z-index: 1;
    opacity: 0.7;
    animation: flicker 2s infinite ease-in-out alternate;
  }
  .flame1 {
    background: radial-gradient(circle, rgba(255,140,0,0.8) 20%, transparent 70%);
    animation-delay: 0s;
  }
  .flame2 {
    background: radial-gradient(circle, rgba(255,69,0,0.8) 15%, transparent 80%);
    animation-delay: 0.5s;
  }
  .flame3 {
    background: radial-gradient(circle, rgba(255,215,0,0.7) 10%, transparent 80%);
    animation-delay: 1s;
  }

  @keyframes flicker {
    0% { transform: scale(1) rotate(0deg); opacity: 0.7; }
    50% { transform: scale(1.2) rotate(12deg); opacity: 1; }
    100% { transform: scale(1) rotate(-12deg); opacity: 0.6; }
  }

  /* Sparks (percikan api) */
  .spark {
    position: absolute;
    width: 6px; height: 6px;
    background: radial-gradient(circle, #ffec85 20%, #ff9900 70%, transparent 100%);
    border-radius: 50%;
    animation: sparkFly 3s linear infinite;
    opacity: 0.8;
    z-index: 2;
  }
  .spark:nth-child(4) { top: 10%; left: 40%; animation-delay: 0s; }
  .spark:nth-child(5) { top: 30%; left: 70%; animation-delay: 0.5s; }
  .spark:nth-child(6) { top: 60%; left: 20%; animation-delay: 1s; }
  .spark:nth-child(7) { top: 80%; left: 50%; animation-delay: 1.5s; }
  .spark:nth-child(8) { top: 40%; left: 10%; animation-delay: 2s; }

  @keyframes sparkFly {
    0% { transform: translateY(0) scale(1); opacity: 0.9; }
    50% { transform: translateY(-40px) scale(0.8); opacity: 1; }
    100% { transform: translateY(-80px) scale(0.5); opacity: 0; }
  }

  /* Tombol oval horizontal */
  .btn-row {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 10px;
  }
  .btn {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 8px 14px;
    border-radius: 25px;
    font-size: 0.9rem;
    font-weight: 600;
    color: #fff;
    text-decoration: none;
    position: relative;
    overflow: hidden;
    z-index: 1;
  }
  .btn::before {
    content: &quot;&quot;;
    position: absolute;
    inset: -2px;
    border-radius: 30px;
    background: linear-gradient(90deg, red, orange, yellow, green, blue, violet);
    background-size: 400% 100%;
    animation: rainbowMove 6s linear infinite;
    z-index: -2;
  }
  .btn::after {
    content: &quot;&quot;;
    position: absolute;
    inset: 2px;
    border-radius: 25px;
    z-index: -1;
  }
  .btn-wa::after { background: #25D366; }
  .btn-yt::after { background: #FF0000; }
  .btn-ig::after { background: linear-gradient(45deg,#f58529,#dd2a7b,#8134af,#515bd4); }
  .btn-fb::after { background: #1877F2; }
  .btn-pin::after { background: #E60023; }
  .btn-th::after { background: #000; }

  .btn i { font-size: 1rem; }

  @keyframes rainbowMove {
    0% { background-position: 0% 50%; }
    100% { background-position: 100% 50%; }
  }
</style>
<!-- Monyet Sawit Widget - Dengan Pesan Motivasi -->
<style>
.aleo-monkey-widget {
  position: fixed; bottom: 12px; right: 12px; width: 78px; height: 84px;
  z-index: 99999; cursor: pointer; transition: transform 0.3s;
}
.aleo-monkey-widget:hover {transform: scale(1.18) rotate(-10deg);}
.monkey-body {
  position: absolute; bottom: 5px; left: 15px; width: 50px; height: 45px;
  background: #d35400; border-radius: 50% 50% 30% 30% / 60% 60% 40% 40%;
  border: 3px solid #a0522d; animation: monkeyDance 2s infinite;
}
@keyframes monkeyDance {
  0%,100%{transform:rotate(-3deg) scale(1);}
  25%{transform:rotate(3deg) scale(1.03);}
  75%{transform:rotate(-5deg) scale(0.97);}
}
.monkey-head {
  position: absolute; top: 5px; left: 20px; width: 45px; height: 47px;
  background: #d35400; border-radius: 50% 50% 40% 40% / 65% 65% 35% 35%;
  border: 3px solid #a0522d; display: flex; align-items: center; justify-content: center;
  font-size: 25px; animation: headBob 1.5s infinite;
}
@keyframes headBob {0%,100%{transform:translateY(0);}50%{transform:translateY(-7px);}}
.monkey-arms {position: absolute; top: 26px; left: 4px; display: flex; gap: 50px;}
.monkey-arms .arm {
  width: 18px; height: 25px; background: #d35400; border: 2px solid #a0522d;
  border-radius: 50px; transform-origin: top center;
}
.monkey-arms .arm.left {animation: armSwing 1.2s infinite;}
.monkey-arms .arm.right {animation: armSwing 1.2s infinite reverse;}
@keyframes armSwing {0%,100%{transform:rotate(-20deg);}50%{transform:rotate(25deg);}}
.coconut {
  position: absolute; top: -5px; right: 5px; font-size: 15px;
  animation: coconutSpin 2.5s infinite; color: #8b4513;
}
@keyframes coconutSpin {0%,100%{transform:rotate(0deg);}100%{transform:rotate(360deg);}}
.monkey-speech {
  position: absolute; right: 85px; bottom: 35px; background: #e74c3c;
  color: white; border-radius: 20px; font-size: 10px; padding: 7px 12px;
  font-family: Arial, sans-serif; white-space: nowrap; opacity: 0;
  transition: opacity 0.4s; box-shadow: 0 3px 15px rgba(231,76,60,0.4);
  animation: speechWiggle 3s infinite;
  pointer-events: none;
}
@keyframes speechWiggle {0%,100%{transform:rotate(0);}25%{transform:rotate(-2deg);}75%{transform:rotate(2deg);}}
.monkey-speech:after {
  content: &#39;&#39;; position: absolute; left: 100%; top: 50%;
  transform: translateY(-50%); border: 5px solid transparent; border-left-color: #e74c3c;
}
.aleo-monkey-widget:hover .monkey-speech {opacity: 1;}
</style>
<div class='aleo-monkey-widget' onclick='window.open(&apos;https://www.youtube.com/@Daily_vlog_anak_rantau?sub_confirmation=1&apos;,&apos;_blank&apos;)'>
  <div class='monkey-body'/>
  <div class='monkey-head'>🐵</div>
  <div class='monkey-arms'>
    <div class='arm left'/>
    <div class='arm right'/>
  </div>
  <div class='banana'>🍌</div>
  <div class='monkey-speech' id='monkeySpeech'>Ayo nonton vlog aku dong!</div>
</div>
<script>
const monkeyPhrases = {
  id: [
    &#39;Ayo nonton vlog aku dong!&#39;,
    &#39;Subscribe dong!&#39;,
    &#39;Ikuti petualangan saya di sawit!&#39;,
    &#39;Jangan lupa like ya Tuan!&#39;,
    &#39;Tolong bantu aku jadi orang sukses 🙏&#39;
  ],
  en: [
    &#39;Watch my vlogs!&#39;,
    &#39;Please subscribe!&#39;,
    &#39;Follow my palm oil adventures!&#39;,
    &#39;Don\&#39;t forget to like!&#39;,
    &#39;Please help me become successful 🙏&#39;
  ],
  ms: [
    &#39;Tengok vlog saya!&#39;,
    &#39;Subscribe lah!&#39;,
    &#39;Ikut adventure sawit!&#39;,
    &#39;Jangan lupa like!&#39;,
    &#39;Tolong bantu saya jadi orang berjaya 🙏&#39;
  ]
};
let phraseIndex = 0;
const lang = (navigator.language||&#39;id&#39;).split(&#39;-&#39;)[0];
const phrases = monkeyPhrases[lang] || monkeyPhrases[&#39;id&#39;];
setInterval(() =&gt; {
  document.getElementById(&#39;monkeySpeech&#39;).textContent = phrases[phraseIndex];
  phraseIndex = (phraseIndex + 1) % phrases.length;
}, 3000);
</script>
    </header>
    <header>
    <div class='header-outer'>
    <div class='header-cap-top cap-top'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    <div class='fauxborder-left header-fauxborder-left'>
    <div class='fauxborder-right header-fauxborder-right'/>
    <div class='region-inner header-inner'>
      <b:section class='header' id='header' maxwidgets='1' name='Header' showaddelement='no'>
        <b:widget id='Header1' locked='false' title='𝔸𝕝𝕖𝕠&apos;𝕤 𝕋𝕦𝕓𝕖 𝔻𝕒𝕚𝕝𝕪 𝕍𝕝𝕠𝕘 𝔸𝕟𝕒𝕜 ℝ𝕒𝕟𝕥𝕒𝕦 𝕀𝕟𝕕𝕠𝕟𝕖𝕤𝕚𝕒 (Header)' type='Header'>
          <b:widget-settings>
            <b:widget-setting name='displayUrl'><![CDATA[https://scontent.fbpn1-1.fna.fbcdn.net/v/t39.30808-6/559105231_122149167692730597_2061348022516593216_n.png?stp=dst-png_s960x960&_nc_cat=109&ccb=1-7&_nc_sid=cc71e4&_nc_eui2=AeEy9lZiG09qzS9ld147DjQZj1hVJ7TV-tuPWFUntNX62-Yogge5vo12THhfaEURlVNd8Fk3HClOcOXI-LxikJlV&_nc_ohc=YvboTByhQ_sQ7kNvwEct62w&_nc_oc=AdlcNgcJA2o3tgWl6kxEezxgQrRox16dunY7tiwvR75lLeyZS6Qr3n0xoBW9gunoRqI&_nc_zt=23&_nc_ht=scontent.fbpn1-1.fna&_nc_gid=MBu_OsNjXfeTp27pSL4qmQ&oh=00_AfdGtX0ew2G_QtePmVfadgDC2-rY7eyet3yPe7mtyyPlQg&oe=68F4FC15]]></b:widget-setting>
            <b:widget-setting name='displayHeight'>423</b:widget-setting>
            <b:widget-setting name='sectionWidth'>752</b:widget-setting>
            <b:widget-setting name='useImage'>true</b:widget-setting>
            <b:widget-setting name='shrinkToFit'>true</b:widget-setting>
            <b:widget-setting name='imagePlacement'>BEFORE_DESCRIPTION</b:widget-setting>
            <b:widget-setting name='displayWidth'>752</b:widget-setting>
          </b:widget-settings>
          <b:includable id='main'>

  <b:if cond='data:useImage'>
    <b:if cond='data:imagePlacement == &quot;BEHIND&quot;'>
      <!--
      Show image as background to text. You can't really calculate the width
      reliably in JS because margins are not taken into account by any of
      clientWidth, offsetWidth or scrollWidth, so we don't force a minimum
      width if the user is using shrink to fit.
      This results in a margin-width's worth of pixels being cropped. If the
      user is not using shrink to fit then we expand the header.
      -->
      <b:if cond='data:mobile'>
        <div id='header-inner'>
          <div class='titlewrapper' style='background: transparent'>
            <h1 class='title' style='background: transparent; border-width: 0px'>
              <b:include name='title'/>
            </h1>
          </div>
          <b:include name='description'/>
        </div>
      <b:else/>
        <div expr:style='&quot;background-image: url(\&quot;&quot; + data:sourceUrl + &quot;\&quot;); &quot;                      + &quot;background-position: &quot;                      + data:backgroundPositionStyleStr + &quot;; &quot;                      + data:widthStyleStr                      + &quot;min-height: &quot; + data:height                      + &quot;_height: &quot; + data:height                      + &quot;background-repeat: no-repeat; &quot;' id='header-inner'>
          <div class='titlewrapper' style='background: transparent'>
            <h1 class='title' style='background: transparent; border-width: 0px'>
              <b:include name='title'/>
            </h1>
          </div>
          <b:include name='description'/>
        </div>
      </b:if>
    <b:else/>
      <!--Show the image only-->
      <div id='header-inner'>
        <a expr:href='data:blog.homepageUrl' style='display: block'>
          <img expr:alt='data:title' expr:height='data:height' expr:id='data:widget.instanceId + &quot;_headerimg&quot;' expr:src='data:sourceUrl' expr:width='data:width' style='display: block'/>
        </a>
        <!--Show the description-->
        <b:if cond='data:imagePlacement == &quot;BEFORE_DESCRIPTION&quot;'>
          <b:include name='description'/>
        </b:if>
      </div>
    </b:if>
  <b:else/>
    <!--No header image -->
    <div id='header-inner'>
      <div class='titlewrapper'>
        <h1 class='title'>
          <b:include name='title'/>
        </h1>
      </div>
      <b:include name='description'/>
    </div>
  </b:if>
</b:includable>
          <b:includable id='description'>
  <div class='descriptionwrapper'>
    <p class='description'><span><data:description/></span></p>
  </div>
</b:includable>
          <b:includable id='title'>
  <b:tag cond='data:blog.url != data:blog.homepageUrl' expr:href='data:blog.homepageUrl' name='a'>
    <data:title/>
  </b:tag>
</b:includable>
        </b:widget>
      </b:section>
    </div>
    </div>
    <div class='header-cap-bottom cap-bottom'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    </div>
    </header>

    <div class='tabs-outer'>
    <div class='tabs-cap-top cap-top'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    <div class='fauxborder-left tabs-fauxborder-left'>
    <div class='fauxborder-right tabs-fauxborder-right'/>
    <div class='region-inner tabs-inner'>
      <b:section class='tabs' id='crosscol' maxwidgets='1' name='Cross-Column' showaddelement='yes'>
        <b:widget id='HTML2' locked='false' title='' type='HTML'>
          <b:widget-settings>
            <b:widget-setting name='content'><![CDATA[<!-- ===============================
🏠 HOMEPAGE ALEO’S TUBE
================================ -->
<b:if cond='data:blog.pageType == "index"'>
  Aleo’s Tube — Daily Vlog Anak Rantau
  
  
  
  <!-- Hero section -->
  <div class="hero">
    <h1>🌏 Aleo’s Tube Daily Vlog Anak Rantau</h1>
    <p>Gabungan cerita kehidupan, tutorial blogger, dan tools otomatis buatan Aleo.</p>
  </div>
</b:if>
<!-- Linktree Horizontal Oval dengan Avatar Efek Api + Spark -->
<div class="linktree-mini">
  <div class="avatar-fire">
    <img src="https://ugc.production.linktr.ee/6594230f-65d4-4070-9815-dba010c4b2b9_Aleos-Tube.png?io=true&size=avatar-v3_0" alt="Aleo's Tube" id="avatar" />
    <div class="flame flame1"></div>
    <div class="flame flame2"></div>
    <div class="flame flame3"></div>

   <!-- Sparks -->
    <span class="spark"></span>
    <span class="spark"></span>
    <span class="spark"></span>
    <span class="spark"></span>
    <span class="spark"></span>
  </div>O
    <a href="https://youtube.com/UCTHGlP7T12oHv2QHDuw0C3g" class="btn btn-yt"><i class="fab fa-youtube"></i> YT</a>
    <a href="https://www.instagram.com/daily_vlog_anak_rantau" class="btn btn-ig"><i class="fab fa-instagram"></i> IG</a>
    <a 
href="https://facebook.com/aleostube" class="btn btn-fb"><i class="fab fa-facebook"></i> FB</a>
    <a 
href="https://pinterest.com/aleostube" class="btn btn-pin"><i class="fab fa-pinterest"></i> Pin</a>
    <a 
href="https://x.com/aleostube" class="btn btn-th"><i class="fab fa-x"></i> XT</a>
  <div class="btn-row">
</div>
</div>

<!-- =================== ALEO’S TUBE BLOG LIGHT INTERACTIVE =================== -->
<style type="text/css">
#welcome-message {
  position: fixed;
  top: 20%; left: 50%;
  transform: translateX(-50%);
  background: rgba(255,255,255,0.9);
  padding: 15px 30px;
  border-radius: 15px;
  font-family: Poppins,sans-serif;
  font-size: 1.1rem;
  color: #222;
  text-align: center;
  opacity: 0;
  z-index: 9999;
  animation: fadeInOut 6s ease-in-out forwards;
  box-shadow:0 3px 10px rgba(0,0,0,0.15);
}
@keyframes fadeInOut {
  0% {opacity:0; transform:translate(-50%,-10%);}
  15% {opacity:1; transform:translate(-50%,0);}
  85% {opacity:1; transform:translate(-50%,0);}
  100% {opacity:0; transform:translate(-50%,10%);}
}
body.dark-mode #welcome-message { background: rgba(20,20,30,0.85); color:#fafafa; }
</style>

<script type="text/javascript">
//<![CDATA[
window.addEventListener('load', () => {
  // Dark mode otomatis
  const hour = new Date().getHours();
  if(hour >= 18 || hour < 6) document.body.classList.add("dark-mode");

  // Multi-bahasa
  const userLang = (navigator.language || 'id').split('-')[0];
  const messages = {
    id: ["Selamat Datang di blog saya!","Halo! Terima kasih sudah mampir!"],
    en: ["Welcome to my blog!","Hello! Thanks for visiting!"],
    es: ["¡Bienvenidos a mi blog!","Hola! Gracias por visitar!"],
    fr: ["Bienvenue sur mon blog!","Salut! Merci pour votre visite!"],
    de: ["Willkommen auf meinem Blog!","Hallo! Danke für den Besuch!"],
    ja: ["私のブログへようこそ！","こんにちは! ご訪問ありがとうございます。"],
    zh: ["欢迎来到我的博客！","你好! 感谢访问!"],
    ko: ["제 블로그에 오신 것을 환영합니다!","안녕하세요! 방문해 주셔서 감사합니다!"],
    tet: ["Bondia senhór no senhóra sira!","Hola! Favor fahi blog hau!"]
  };

  const greetArray = messages[userLang] || messages.id;
  const greeting = greetArray[Math.floor(Math.random() * greetArray.length)];

  // tampilkan pesan
  const msgBox = document.createElement("div");
  msgBox.id = "welcome-message";
  msgBox.innerText = greeting;
  document.body.appendChild(msgBox);

  // suara otomatis
  setTimeout(()=>{
    try{
      const synth = window.speechSynthesis;
      const utter = new SpeechSynthesisUtterance(greeting);
      const langMap = { tet:'pt-PT' };
      utter.lang = langMap[userLang] || (userLang+'-'+userLang.toUpperCase());
      utter.pitch=1; utter.rate=1; utter.volume=1;
      synth.speak(utter);
    }catch(e){}
  },500);

  // hapus pesan setelah animasi
  setTimeout(()=> msgBox.remove(),6000);
});
//]]]]><![CDATA[>
</script>

<!-- 🌍 ALEO'S TUBE - Auto SEO Fixer + Stylish Affiliate Buttons -->
<style>
/* 🎨 Gaya Tombol Profesional */
.btn {
  display: inline-block;
  margin: 8px;
  padding: 12px 22px;
  font-size: 16px;
  font-weight: 600;
  text-decoration: none;
  color: #fff;
  border-radius: 12px;
  transition: all 0.25s ease-in-out;
  background: linear-gradient(135deg, #007bff, #00bfff);
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}

.btn:hover {
  transform: translateY(-2px);
  background: linear-gradient(135deg, #0056b3, #0099cc);
  box-shadow: 0 6px 14px rgba(0,0,0,0.25);
}

.btn:active {
  transform: scale(0.97);
}

@media (max-width: 600px) {
  .btn {
    display: block;
    width: 100%;
    text-align: center;
    box-sizing: border-box;
  }
}
</style>

<!-- 🚀 Auto SEO Fixer untuk Tombol Afiliasi -->
<script>
document.addEventListener("DOMContentLoaded", function() {
  const links = document.querySelectorAll("a[href='javascript:void(0);'][onclick*='trackAffiliate']");
  
  links.forEach(a => {
    const onclickAttr = a.getAttribute("onclick");
    const match = onclickAttr.match(/trackAffiliate\(['"]([^'"]+)['"],['"]([^'"]+)['"]\)/);
    
    if (match) {
      const productName = match[1];
      const targetUrl = match[2];
      
      // Ubah jadi SEO-friendly link
      a.setAttribute("href", targetUrl);
      a.setAttribute("onclick", `trackAffiliate('${productName}', this.href); return false;`);
      a.setAttribute("rel", "nofollow noopener noreferrer");
      
      // Tambah kelas tombol (kalau belum ada)
      if (!a.classList.contains("btn")) a.classList.add("btn");
      
      console.log("✅ Diperbaiki:", productName, "→", targetUrl);
    }
  });
});
</script>

<!-- 📊 Fungsi Tracking -->
<script>
function trackAffiliate(productName, url) {
  console.log("📈 Tracking:", productName);
  // kamu bisa tambahkan pelacakan custom di sini (<!-- 🔹 Google Analytics 4 (Aleo’s Tube) -->
<script async="async" src="https://www.googletagmanager.com/gtag/js?id=G-71BR09BJ1R"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-71BR09BJ1R');
</script>
, <!-- Meta Pixel Code (Safe for Blogger) -->
<b:if cond='data:blog.pageType != &quot;preview&quot;'>
<script type='text/javascript'>
//<![CDATA[
!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};
if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];
s.parentNode.insertBefore(t,s)}(window, document,'script',
'https://connect.facebook.net/en_US/fbevents.js');
fbq('init', '943661851197731');
fbq('track', 'PageView');
//]]]]><![CDATA[>
</script>
<noscript>
  <img height='1' width='1' style='display:none'
       src='https://www.facebook.com/tr?id=943661851197731&amp;ev=PageView&amp;noscript=1'/>
</noscript>
</b:if>
<!-- End Meta Pixel Code -->
, dll)
  window.location.href = url; // redirect ke link tujuan
}
<!-- 💎 ALEO’S TUBE — AUTO PAGESPEED SCORE v2.5 ANIMATED -->
<div id="aleos-pageSpeed" style="font-family:'Poppins',sans-serif;text-align:center;padding:20px;border:2px solid #00bfa5;border-radius:16px;box-shadow:0 0 10px rgba(0,0,0,0.1);max-width:380px;margin:auto;">
  <h3 style="margin-bottom:10px;">🚀 Skor Kecepatan Blog</h3>
  <div style="margin-bottom:15px;">
    <div style="font-weight:bold;">Mobile</div>
    <div class="progress-bar" id="mobileBar" style="height:14px;width:100%;background:#ddd;border-radius:8px;overflow:hidden;">
      <div id="mobileFill" style="height:100%;width:0;background:#00bfa5;transition:width 2s ease;"></div>
    </div>
    <div id="mobileScore" style="margin-top:5px;">Memuat...</div>
  </div>

  <div>
    <div style="font-weight:bold;">Desktop</div>
    <div class="progress-bar" id="desktopBar" style="height:14px;width:100%;background:#ddd;border-radius:8px;overflow:hidden;">
      <div id="desktopFill" style="height:100%;width:0;background:#00bfa5;transition:width 2s ease;"></div>
    </div>
    <div id="desktopScore" style="margin-top:5px;">Memuat...</div>
  </div>

  <small style="display:block;margin-top:15px;">
    🔗 <a href="https://pagespeed.web.dev/analysis/https-aleos-tube-daily-vlog-anak-rantau-blogspot-com" target="_blank">Lihat detail di PageSpeed Insights</a>
  </small>
</div>

<script>
(async function(){
  const blogUrl = 'https://aleos-tube-daily-vlog-anak-rantau.blogspot.com';

  async function fetchScore(strategy){
    const api = `https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=${blogUrl}&strategy=${strategy}`;
    const res = await fetch(api);
    const data = await res.json();
    return Math.round(data.lighthouseResult.categories.performance.score * 100);
  }

  function colorFor(score){
    if(score >= 90) return '#00c853'; // Hijau
    if(score >= 50) return '#ffb300'; // Oranye
    return '#d50000'; // Merah
  }

  async function updateUI(device){
    const score = await fetchScore(device);
    const fill = document.getElementById(device + 'Fill');
    const bar = document.getElementById(device + 'Bar');
    const text = document.getElementById(device + 'Score');
    fill.style.width = score + '%';
    fill.style.background = colorFor(score);
    text.innerHTML = `<b>${score}/100</b> (${device})`;
    text.style.color = colorFor(score);
  }

  updateUI('mobile');
  updateUI('desktop');
})();
</script>

<!-- 💎 ALEO’S TUBE — AUTO PAGESPEED SCORE v2.5 ANIMATED -->
<div id="aleos-pageSpeed" style="font-family:'Poppins',sans-serif;text-align:center;padding:20px;border:2px solid #00bfa5;border-radius:16px;box-shadow:0 0 10px rgba(0,0,0,0.1);max-width:380px;margin:auto;">
  <h3 style="margin-bottom:10px;">🚀 Skor Kecepatan Blog</h3>
  <div style="margin-bottom:15px;">
    <div style="font-weight:bold;">Mobile</div>
    <div class="progress-bar" id="mobileBar" style="height:14px;width:100%;background:#ddd;border-radius:8px;overflow:hidden;">
      <div id="mobileFill" style="height:100%;width:0;background:#00bfa5;transition:width 2s ease;"></div>
    </div>
    <div id="mobileScore" style="margin-top:5px;">Memuat...</div>
  </div>

  <div>
    <div style="font-weight:bold;">Desktop</div>
    <div class="progress-bar" id="desktopBar" style="height:14px;width:100%;background:#ddd;border-radius:8px;overflow:hidden;">
      <div id="desktopFill" style="height:100%;width:0;background:#00bfa5;transition:width 2s ease;"></div>
    </div>
    <div id="desktopScore" style="margin-top:5px;">Memuat...</div>
  </div>

  <small style="display:block;margin-top:15px;">
    🔗 <a href="https://pagespeed.web.dev/analysis/https-aleos-tube-daily-vlog-anak-rantau-blogspot-com" target="_blank">Lihat detail di PageSpeed Insights</a>
  </small>
</div>

<script>
(async function(){
  const blogUrl = 'https://aleos-tube-daily-vlog-anak-rantau.blogspot.com';

  async function fetchScore(strategy){
    const api = `https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=${blogUrl}&strategy=${strategy}`;
    const res = await fetch(api);
    const data = await res.json();
    return Math.round(data.lighthouseResult.categories.performance.score * 100);
  }

  function colorFor(score){
    if(score >= 90) return '#00c853'; // Hijau
    if(score >= 50) return '#ffb300'; // Oranye
    return '#d50000'; // Merah
  }

  async function updateUI(device){
    const score = await fetchScore(device);
    const fill = document.getElementById(device + 'Fill');
    const bar = document.getElementById(device + 'Bar');
    const text = document.getElementById(device + 'Score');
    fill.style.width = score + '%';
    fill.style.background = colorFor(score);
    text.innerHTML = `<b>${score}/100</b> (${device})`;
    text.style.color = colorFor(score);
  }

  updateUI('mobile');
  updateUI('desktop');
})();
</script>
<!-- SEO STATUS DASHBOARD for Aleo's Tube -->
<!doctype html>


  
  
  SEO Status — Aleo's Tube
  <style>
    body{font-family:Segoe UI, Roboto, Arial; background:#f6f8fb; color:#0b1220; padding:28px;}
    .card{background:white;border-radius:12px;padding:18px;box-shadow:0 8px 28px rgba(13,41,77,0.06);max-width:920px;margin:0 auto;}
    h1{margin:0 0 12px;font-size:20px}
    .row{display:flex;gap:12px;align-items:center;margin-top:10px;flex-wrap:wrap}
    .badge{padding:8px 12px;border-radius:10px;background:linear-gradient(90deg,#ff6b00,#0077ff);color:#fff;font-weight:700}
    button{padding:8px 12px;border-radius:10px;border:none;background:#0077ff;color:#fff;cursor:pointer;font-weight:700}
    .muted{color:#5b6782;font-size:13px}
    pre{background:#0b1220;color:#dff4ff;padding:12px;border-radius:8px;overflow:auto}
  </style>


  <div class="card">
    <h1>SEO Status — Aleo's Tube</h1>
    <div class="muted">Halaman ini menampilkan status ping sitemap &amp; consent lokal. Semua tindakan bersifat client-side dan tidak memerlukan akses server.</div>

    <div class="row" style="margin-top:18px">
      <div>
        <div class="muted">Consent analytics</div>
        <div id="consentStatus" class="badge">—</div>
      </div>

      <div>
        <div class="muted">Last auto-ping</div>
        <div id="lastPing" style="padding:8px 12px;border-radius:8px;background:#f1f5f9;color:#0b1220;font-weight:700">—</div>
      </div>

      <div style="margin-left:auto; display:flex; gap:8px;">
        <button id="manualPing">Ping Sekarang</button>
        <button id="clearPing" style="background:#ff4d4f">Reset</button>
      </div>
    </div>

    <section style="margin-top:18px;">
      <h3 style="margin:8px 0">Log Ringkas</h3>
      <div class="muted">Log di bawah hanya untuk debugging &amp; tersimpan sementara di sessionStorage.</div>
      <pre id="logArea" style="height:160px">—</pre>
    </section>

    <section style="margin-top:16px;">
      <small class="muted">Catatan: Ping dilakukan via <code>fetch</code> ke endpoint publik (Google/Bing) dengan mode <code>no-cors</code>. Browser tidak menampilkan respons yang bisa dibaca — operasi bersifat "fire-and-forget".</small>
    </section>
  </div>

  <script>
    (function(){
      var pingKey = 'gt_ping_v10_1';
      var consentKey = 'gt_accept_v10_1';
      var logArea = document.getElementById('logArea');

      function appendLog(msg){
        try {
          var now = new Date().toLocaleString();
          var prev = sessionStorage.getItem('aleos_seo_log') || '';
          var line = '['+ now +'] ' + msg + '\\n';
          prev = line + prev;
          sessionStorage.setItem('aleos_seo_log', prev.slice(0, 5000));
          logArea.textContent = prev || '—';
        } catch(e){ console.warn(e); }
      }

      function updateUI(){
        var last = localStorage.getItem(pingKey);
        document.getElementById('lastPing').textContent = last ? new Date(parseInt(last,10)).toLocaleString() : 'Belum pernah';
        document.getElementById('consentStatus').textContent = localStorage.getItem(consentKey) ? 'Diterima' : 'Belum';
      }

      function pingNow(){
        try {
          var sitemap = location.origin + '/sitemap.xml';
          appendLog('Mencoba ping Google & Bing untuk: ' + sitemap);
          // Google
          fetch('https://www.google.com/ping?sitemap=' + encodeURIComponent(sitemap), {mode:'no-cors'}).then(function(){ appendLog('Ping Google dikirim'); }).catch(function(e){ appendLog('Ping Google gagal'); });
          // Bing
          fetch('https://www.bing.com/ping?sitemap=' + encodeURIComponent(sitemap), {mode:'no-cors'}).then(function(){ appendLog('Ping Bing dikirim'); }).catch(function(e){ appendLog('Ping Bing gagal'); });
          localStorage.setItem(pingKey, String(Date.now()));
          updateUI();
        } catch(e){
          appendLog('Error ping: ' + (e && e.message ? e.message : e));
        }
      }

      document.getElementById('manualPing').addEventListener('click', function(){ pingNow(); }, {passive:true});
      document.getElementById('clearPing').addEventListener('click', function(){
        localStorage.removeItem(pingKey);
        sessionStorage.removeItem('aleos_seo_log');
        logArea.textContent = '—';
        updateUI();
      }, {passive:true});

      // init
      try {
        logArea.textContent = sessionStorage.getItem('aleos_seo_log') || '—';
        updateUI();
        appendLog('Dashboard dibuka');
      } catch(e){ console.warn(e); }
    })();
  </script>

</!doctype>]]></b:widget-setting>
          </b:widget-settings>
          <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
        </b:widget>
      </b:section>
      <b:section class='tabs' id='crosscol-overflow' name='Cross-Column 2' showaddelement='no'/>
    </div>
    </div>
    <div class='tabs-cap-bottom cap-bottom'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    </div>

    <div class='main-outer'>
    <div class='main-cap-top cap-top'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>

    <div class='fauxborder-left main-fauxborder-left'>
    <div class='fauxborder-right main-fauxborder-right'/>
    <div class='region-inner main-inner'>

      <div class='columns fauxcolumns'>

        <div class='fauxcolumn-outer fauxcolumn-center-outer'>
        <div class='cap-top'>
          <div class='cap-left'/>
          <div class='cap-right'/>
        </div>
        <div class='fauxborder-left'>
        <div class='fauxborder-right'/>
        <div class='fauxcolumn-inner'>
        </div>
        </div>
        <div class='cap-bottom'>
          <div class='cap-left'/>
          <div class='cap-right'/>
        </div>
        </div>

        <div class='fauxcolumn-outer fauxcolumn-left-outer'>
        <div class='cap-top'>
          <div class='cap-left'/>
          <div class='cap-right'/>
        </div>
        <div class='fauxborder-left'>
        <div class='fauxborder-right'/>
        <div class='fauxcolumn-inner'>
        </div>
        </div>
        <div class='cap-bottom'>
          <div class='cap-left'/>
          <div class='cap-right'/>
        </div>
        </div>

        <div class='fauxcolumn-outer fauxcolumn-right-outer'>
        <div class='cap-top'>
          <div class='cap-left'/>
          <div class='cap-right'/>
        </div>
        <div class='fauxborder-left'>
        <div class='fauxborder-right'/>
        <div class='fauxcolumn-inner'>
        </div>
        </div>
        <div class='cap-bottom'>
          <div class='cap-left'/>
          <div class='cap-right'/>
        </div>
        </div>

        <!-- corrects IE6 width calculation -->
        <div class='columns-inner'>

        <div class='column-center-outer'>
        <div class='column-center-inner'>
          <b:section class='main' id='main' name='Main' showaddelement='no'>
            <b:widget id='Blog1' locked='false' title='Postingan Blog' type='Blog' version='1'>
              <b:widget-settings>
                <b:widget-setting name='commentLabel'>comments</b:widget-setting>
                <b:widget-setting name='showShareButtons'>true</b:widget-setting>
                <b:widget-setting name='authorLabel'>By Aleo</b:widget-setting>
                <b:widget-setting name='style.unittype'>TextAndImage</b:widget-setting>
                <b:widget-setting name='timestampLabel'>at</b:widget-setting>
                <b:widget-setting name='reactionsLabel'/>
                <b:widget-setting name='showAuthorProfile'>true</b:widget-setting>
                <b:widget-setting name='style.layout'>1x1</b:widget-setting>
                <b:widget-setting name='showLocation'>false</b:widget-setting>
                <b:widget-setting name='showTimestamp'>true</b:widget-setting>
                <b:widget-setting name='postsPerAd'>1</b:widget-setting>
                <b:widget-setting name='style.bordercolor'>#ffffff</b:widget-setting>
                <b:widget-setting name='showDateHeader'>true</b:widget-setting>
                <b:widget-setting name='style.textcolor'>#ffffff</b:widget-setting>
                <b:widget-setting name='showCommentLink'>true</b:widget-setting>
                <b:widget-setting name='style.urlcolor'>#ffffff</b:widget-setting>
                <b:widget-setting name='postLocationLabel'>Location: Indonesia</b:widget-setting>
                <b:widget-setting name='showAuthor'>true</b:widget-setting>
                <b:widget-setting name='style.linkcolor'>#ffffff</b:widget-setting>
                <b:widget-setting name='style.bgcolor'>#ffffff</b:widget-setting>
                <b:widget-setting name='showLabels'>false</b:widget-setting>
                <b:widget-setting name='postLabelsLabel'>Labels: Daily Vlog Anak Rantau Indonesia,</b:widget-setting>
                <b:widget-setting name='showBacklinks'>false</b:widget-setting>
                <b:widget-setting name='showInlineAds'>false</b:widget-setting>
                <b:widget-setting name='showReactions'>false</b:widget-setting>
              </b:widget-settings>
              <b:includable id='main' var='top'>
  <b:if cond='!data:mobile'>
    <!-- posts -->
    <div class='blog-posts hfeed'>

      <b:include data='top' name='status-message'/>

      <b:loop values='data:posts' var='post'>
        <b:if cond='data:post.isDateStart and not data:post.isFirstPost'>
          &lt;/div&gt;&lt;/div&gt;
        </b:if>
        <b:if cond='data:post.isDateStart'>
          &lt;div class=&quot;date-outer&quot;&gt;
        </b:if>
        <b:if cond='data:post.dateHeader'>
          <h2 class='date-header'><span><data:post.dateHeader/></span></h2>
        </b:if>
        <b:if cond='data:post.isDateStart'>
          &lt;div class=&quot;date-posts&quot;&gt;
        </b:if>
        <div class='post-outer'>
          <b:include data='post' name='post'/>
          <b:include cond='data:blog.pageType in {&quot;static_page&quot;,&quot;item&quot;}' data='post' name='comment_picker'/>
        </div>

        <!-- Ad -->
        <b:if cond='data:post.includeAd'>
          <div class='inline-ad'>
            <data:adCode/>
          </div>
        </b:if>
      </b:loop>
      <b:if cond='data:numPosts != 0'>
        &lt;/div&gt;&lt;/div&gt;
      </b:if>
    </div>

    <!-- navigation -->
    <b:include name='nextprev'/>

    <!-- feed links -->
    <b:include name='feedLinks'/>

  <b:else/>
    <b:include name='mobile-main'/>
  </b:if>
</b:includable>
              <b:includable id='backlinkDeleteIcon' var='backlink'/>
              <b:includable id='backlinks' var='post'/>
              <b:includable id='comment-form' var='post'>
  <div class='comment-form'>
    <a name='comment-form'/>
    <b:if cond='data:mobile'>
      <h4 id='comment-post-message'>
        <a expr:id='data:widget.instanceId + &quot;_comment-editor-toggle-link&quot;' href='javascript:void(0)'><data:postCommentMsg/></a></h4>
      <p><data:blogCommentMessage/></p>
      <data:blogTeamBlogMessage/>
      <a expr:href='data:post.commentFormIframeSrc' id='comment-editor-src'/>
      <iframe allowtransparency='true' class='blogger-iframe-colorize blogger-comment-from-post' expr:height='data:cmtIframeInitialHeight' frameborder='0' id='comment-editor' name='comment-editor' src='' style='display: none' width='100%'/>
    <b:else/>
      <h4 id='comment-post-message'><data:postCommentMsg/></h4>
      <p><data:blogCommentMessage/></p>
      <data:blogTeamBlogMessage/>
      <a expr:href='data:post.commentFormIframeSrc' id='comment-editor-src'/>
      <iframe allowtransparency='true' class='blogger-iframe-colorize blogger-comment-from-post' expr:height='data:cmtIframeInitialHeight' frameborder='0' id='comment-editor' name='comment-editor' src='' width='100%'/>
    </b:if>
    <data:post.cmtfpIframe/>
    <script type='text/javascript'>
      BLOG_CMT_createIframe(&#39;<data:post.appRpcRelayPath/>&#39;);
    </script>
  </div>
</b:includable>
              <b:includable id='commentDeleteIcon' var='comment'>
  <span expr:class='&quot;item-control &quot; + data:comment.adminClass'>
    <b:if cond='data:showCmtPopup'>
      <div class='goog-toggle-button'>
        <div class='goog-inline-block comment-action-icon'/>
      </div>
    <b:else/>
      <a class='comment-delete' expr:href='data:comment.deleteUrl' expr:title='data:top.deleteCommentMsg'>
        <img src='https://resources.blogblog.com/img/icon_delete13.gif'/>
      </a>
    </b:if>
  </span>
</b:includable>
              <b:includable id='comment_count_picker' var='post'>
  <a class='comment-link' expr:href='data:post.addCommentUrl' expr:onclick='data:post.addCommentOnclick'>
    <data:post.commentLabelFull/>:
  </a>
</b:includable>
              <b:includable id='comment_picker' var='post'>
  <b:if cond='data:post.showThreadedComments'>
    <b:include data='post' name='threaded_comments'/>
  <b:else/>
    <b:include data='post' name='comments'/>
  </b:if>
</b:includable>
              <b:includable id='comments' var='post'>
  <div class='comments' id='comments'>
    <a name='comments'/>
    <b:if cond='data:post.allowComments'>
      <h4><data:post.commentLabelFull/>:</h4>

      <b:if cond='data:post.commentPagingRequired'>
        <span class='paging-control-container'>
          <b:if cond='data:post.hasOlderLinks'>
            <a expr:class='data:post.oldLinkClass' expr:href='data:post.oldestLinkUrl'><data:post.oldestLinkText/></a>
              &#160;
            <a expr:class='data:post.oldLinkClass' expr:href='data:post.olderLinkUrl'><data:post.olderLinkText/></a>
              &#160;
          </b:if>

          <data:post.commentRangeText/>

          <b:if cond='data:post.hasNewerLinks'>
            &#160;
            <a expr:class='data:post.newLinkClass' expr:href='data:post.newerLinkUrl'><data:post.newerLinkText/></a>
            &#160;
            <a expr:class='data:post.newLinkClass' expr:href='data:post.newestLinkUrl'><data:post.newestLinkText/></a>
          </b:if>
        </span>
      </b:if>

      <div expr:id='data:widget.instanceId + &quot;_comments-block-wrapper&quot;'>
        <dl expr:class='data:post.avatarIndentClass' id='comments-block'>
          <b:loop values='data:post.comments' var='comment'>
            <dt expr:class='&quot;comment-author &quot; + data:comment.authorClass' expr:id='data:comment.anchorName'>
              <b:if cond='data:comment.favicon'>
                <img expr:src='data:comment.favicon' height='16px' style='margin-bottom:-2px;' width='16px'/>
              </b:if>
              <a expr:name='data:comment.anchorName'/>
              <b:if cond='data:blog.enabledCommentProfileImages'>
                <data:comment.authorAvatarImage/>
              </b:if>
              <b:if cond='data:comment.authorUrl'>
                <a expr:href='data:comment.authorUrl' rel='nofollow'><data:comment.author/></a>
              <b:else/>
                <data:comment.author/>
              </b:if>
              <data:commentPostedByMsg/>
            </dt>
            <dd class='comment-body' expr:id='data:widget.instanceId + data:comment.cmtBodyIdPostfix'>
              <b:if cond='data:comment.isDeleted'>
                <span class='deleted-comment'><data:comment.body/></span>
              <b:else/>
                <p>
                  <data:comment.body/>
                </p>
              </b:if>
            </dd>
            <dd class='comment-footer'>
              <span class='comment-timestamp'>
                <a expr:href='data:comment.url' title='comment permalink'>
                  <data:comment.timestamp/>
                </a>
                <b:include data='comment' name='commentDeleteIcon'/>
              </span>
            </dd>
          </b:loop>
        </dl>
      </div>

      <b:if cond='data:post.commentPagingRequired'>
        <span class='paging-control-container'>
          <a expr:class='data:post.oldLinkClass' expr:href='data:post.oldestLinkUrl'>
            <data:post.oldestLinkText/>
          </a>
          <a expr:class='data:post.oldLinkClass' expr:href='data:post.olderLinkUrl'>
            <data:post.olderLinkText/>
          </a>
          &#160;
          <data:post.commentRangeText/>
          &#160;
          <a expr:class='data:post.newLinkClass' expr:href='data:post.newerLinkUrl'>
            <data:post.newerLinkText/>
          </a>
          <a expr:class='data:post.newLinkClass' expr:href='data:post.newestLinkUrl'>
            <data:post.newestLinkText/>
          </a>
        </span>
      </b:if>

      <p class='comment-footer'>
        <b:if cond='data:post.embedCommentForm'>
          <b:if cond='data:post.allowNewComments'>
            <b:include data='post' name='comment-form'/>
          <b:else/>
            <data:post.noNewCommentsText/>
          </b:if>
        <b:elseif cond='data:post.allowComments'/>
          <a expr:href='data:post.addCommentUrl' expr:onclick='data:post.addCommentOnclick'><data:postCommentMsg/></a>
        </b:if>
      </p>
    </b:if>
    <b:if cond='data:showCmtPopup'>
      <div id='comment-popup'>
        <iframe allowtransparency='true' frameborder='0' id='comment-actions' name='comment-actions' scrolling='no'>
        </iframe>
      </div>
    </b:if>

  </div>
</b:includable>
              <b:includable id='feedLinks'>
  <b:if cond='data:blog.pageType != &quot;item&quot;'> <!-- Blog feed links -->
    <b:if cond='data:feedLinks'>
      <div class='blog-feeds'>
        <b:include data='feedLinks' name='feedLinksBody'/>
      </div>
    </b:if>

  <b:else/> <!--Post feed links -->
    <div class='post-feeds'>
      <b:loop values='data:posts' var='post'>
        <b:include cond='data:post.allowComments and data:post.feedLinks' data='post.feedLinks' name='feedLinksBody'/>
      </b:loop>
    </div>
  </b:if>
</b:includable>
              <b:includable id='feedLinksBody' var='links'>
  <div class='feed-links'>
  <data:feedLinksMsg/>
  <b:loop values='data:links' var='f'>
     <a class='feed-link' expr:href='data:f.url' expr:type='data:f.mimeType' target='_blank'><data:f.name/> (<data:f.feedType/>)</a>
  </b:loop>
  </div>
</b:includable>
              <b:includable id='iframe_comments' var='post'>
  <!-- G+ comments, no longer available. The includable is retained for backwards-compatibility. -->
</b:includable>
              <b:includable id='mobile-index-post' var='post'>
  <div class='mobile-date-outer date-outer'>
    <b:if cond='data:post.dateHeader'>
      <div class='date-header'>
        <span><data:post.dateHeader/></span>
      </div>
    </b:if>

    <div class='mobile-post-outer'>
      <a expr:href='data:post.url'>
        <h3 class='mobile-index-title entry-title' itemprop='name'>
          <data:post.title/>
        </h3>

        <div class='mobile-index-arrow'>&amp;rsaquo;</div>

        <div class='mobile-index-contents'>
          <b:if cond='data:post.thumbnailUrl'>
            <div class='mobile-index-thumbnail'>
              <div class='Image'>
                <img expr:src='data:post.thumbnailUrl'/>
              </div>
            </div>
          </b:if>

          <div class='post-body'>
            <b:if cond='data:post.snippet'><data:post.snippet/></b:if>
          </div>
        </div>

        <div style='clear: both;'/>
      </a>

      <div class='mobile-index-comment'>
        <b:include cond='data:blog.pageType != &quot;static_page&quot;                          and data:post.allowComments                          and data:post.numComments != 0' data='post' name='comment_count_picker'/>
      </div>
    </div>
  </div>
</b:includable>
              <b:includable id='mobile-main' var='top'>
    <!-- posts -->
    <div class='blog-posts hfeed'>

      <b:include data='top' name='status-message'/>

      <b:if cond='data:blog.pageType == &quot;index&quot;'>
        <b:loop values='data:posts' var='post'>
          <b:include data='post' name='mobile-index-post'/>
        </b:loop>
      <b:else/>
        <b:loop values='data:posts' var='post'>
          <b:include data='post' name='mobile-post'/>
        </b:loop>
      </b:if>
    </div>

   <b:include name='mobile-nextprev'/>
</b:includable>
              <b:includable id='mobile-nextprev'>
  <div class='blog-pager' id='blog-pager'>
    <b:if cond='data:newerPageUrl'>
      <div class='mobile-link-button' id='blog-pager-newer-link'>
      <a class='blog-pager-newer-link' expr:href='data:newerPageUrl' expr:id='data:widget.instanceId + &quot;_blog-pager-newer-link&quot;' expr:title='data:newerPageTitle'>&amp;lsaquo;</a>
      </div>
    </b:if>

    <b:if cond='data:olderPageUrl'>
      <div class='mobile-link-button' id='blog-pager-older-link'>
      <a class='blog-pager-older-link' expr:href='data:olderPageUrl' expr:id='data:widget.instanceId + &quot;_blog-pager-older-link&quot;' expr:title='data:olderPageTitle'>&amp;rsaquo;</a>
      </div>
    </b:if>

    <div class='mobile-link-button' id='blog-pager-home-link'>
    <a class='home-link' expr:href='data:blog.homepageUrl'><data:homeMsg/></a>
    </div>

    <div class='mobile-desktop-link'>
      <a class='home-link' expr:href='data:desktopLinkUrl'><data:desktopLinkMsg/></a>
    </div>

  </div>
  <div class='clear'/>
</b:includable>
              <b:includable id='mobile-post' var='post'>
  <div class='date-outer'>
    <b:if cond='data:post.dateHeader'>
      <h2 class='date-header'><span><data:post.dateHeader/></span></h2>
    </b:if>
    <div class='date-posts'>
      <div class='post-outer'>

        <div class='post hentry uncustomized-post-template' itemscope='itemscope' itemtype='http://schema.org/BlogPosting'>
          <b:if cond='data:post.thumbnailUrl'>
            <meta expr:content='data:post.thumbnailUrl' itemprop='image_url'/>
          </b:if>
          <meta expr:content='data:blog.blogId' itemprop='blogId'/>
          <meta expr:content='data:post.id' itemprop='postId'/>

          <a expr:name='data:post.id'/>
          <b:if cond='data:post.title'>
            <h3 class='post-title entry-title' itemprop='name'>
              <b:if cond='data:post.link'>
                <a expr:href='data:post.link'><data:post.title/></a>
              <b:elseif cond='data:post.url and data:blog.url != data:post.url'/>
                <a expr:href='data:post.url'><data:post.title/></a>
              <b:else/>
                <data:post.title/>
              </b:if>
            </h3>
          </b:if>

          <div class='post-header'>
            <div class='post-header-line-1'/>
          </div>

          <div class='post-body entry-content' expr:id='&quot;post-body-&quot; + data:post.id' itemprop='articleBody'>
            <data:post.body/>
            <div style='clear: both;'/> <!-- clear for photos floats -->
          </div>            

          <div class='post-footer'>
            <div class='post-footer-line post-footer-line-1'>
              <span class='post-author vcard'>
                <b:if cond='data:top.showAuthor'>
                  <b:if cond='data:post.authorProfileUrl'>
                    <span class='fn' itemprop='author' itemscope='itemscope' itemtype='http://schema.org/Person'>
                      <meta expr:content='data:post.authorProfileUrl' itemprop='url'/>
                      <a expr:href='data:post.authorProfileUrl' rel='author' title='author profile'>
                        <span itemprop='name'><data:post.author/></span>
                      </a>
                    </span>
                  <b:else/>
                    <span class='fn' itemprop='author' itemscope='itemscope' itemtype='http://schema.org/Person'>
                      <span itemprop='name'><data:post.author/></span>
                    </span>
                  </b:if>
                </b:if>
              </span>

              <span class='post-timestamp'>
                <b:if cond='data:top.showTimestamp'>
                  <data:top.timestampLabel/>
                  <b:if cond='data:post.url'>
                    <meta expr:content='data:post.url.canonical' itemprop='url'/>
                    <a class='timestamp-link' expr:href='data:post.url' rel='bookmark' title='permanent link'><abbr class='published' expr:title='data:post.timestampISO8601' itemprop='datePublished'><data:post.timestamp/></abbr></a>
                  </b:if>
                </b:if>
              </span>

              <span class='post-comment-link'>
                <b:include cond='data:blog.pageType not in {&quot;item&quot;,&quot;static_page&quot;}                                  and data:post.allowComments' data='post' name='comment_count_picker'/>
              </span>
            </div>

            <div class='post-footer-line post-footer-line-2'>
              <b:if cond='data:top.showMobileShare'>
                <div class='mobile-link-button goog-inline-block' id='mobile-share-button'>
                  <a href='javascript:void(0);'><data:shareMsg/></a>
                </div>
              </b:if>
            </div>

          </div>
        </div>

        <b:include cond='data:blog.pageType in {&quot;static_page&quot;,&quot;item&quot;}' data='post' name='comment_picker'/>
      </div>
    </div>
  </div>
</b:includable>
              <b:includable id='nextprev'>
  <div class='blog-pager' id='blog-pager'>
    <b:if cond='data:newerPageUrl'>
      <span id='blog-pager-newer-link'>
      <a class='blog-pager-newer-link' expr:href='data:newerPageUrl' expr:id='data:widget.instanceId + &quot;_blog-pager-newer-link&quot;' expr:title='data:newerPageTitle'><data:newerPageTitle/></a>
      </span>
    </b:if>

    <b:if cond='data:olderPageUrl'>
      <span id='blog-pager-older-link'>
      <a class='blog-pager-older-link' expr:href='data:olderPageUrl' expr:id='data:widget.instanceId + &quot;_blog-pager-older-link&quot;' expr:title='data:olderPageTitle'><data:olderPageTitle/></a>
      </span>
    </b:if>

    <a class='home-link' expr:href='data:blog.homepageUrl'><data:homeMsg/></a>

    <b:if cond='data:mobileLinkUrl'>
      <div class='blog-mobile-link'>
        <a expr:href='data:mobileLinkUrl'><data:mobileLinkMsg/></a>
      </div>
    </b:if>

  </div>
  <div class='clear'/>
</b:includable>
              <b:includable id='post' var='post'>
  <div class='post hentry uncustomized-post-template' itemprop='blogPost' itemscope='itemscope' itemtype='http://schema.org/BlogPosting'>
    <b:if cond='data:post.firstImageUrl'>
      <meta expr:content='data:post.firstImageUrl' itemprop='image_url'/>
    </b:if>
    <meta expr:content='data:blog.blogId' itemprop='blogId'/>
    <meta expr:content='data:post.id' itemprop='postId'/>

    <a expr:name='data:post.id'/>
    <b:if cond='data:post.title'>
      <h3 class='post-title entry-title' itemprop='name'>
      <b:if cond='data:post.link or (data:post.url and data:blog.url != data:post.url)'>
        <a expr:href='data:post.link ? data:post.link : data:post.url'><data:post.title/></a>
      <b:else/>
        <data:post.title/>
      </b:if>
      </h3>
    </b:if>

    <div class='post-header'>
    <div class='post-header-line-1'/>
    </div>

    <!-- Then use the post body as the schema.org description, for good G+/FB snippeting. -->
    <div class='post-body entry-content' expr:id='&quot;post-body-&quot; + data:post.id' expr:itemprop='(data:blog.metaDescription ? &quot;&quot; : &quot;description &quot;) + &quot;articleBody&quot;'>
      <data:post.body/>
      <div style='clear: both;'/> <!-- clear for photos floats -->
    </div>

    <b:if cond='data:post.hasJumpLink'>
      <div class='jump-link'>
        <a expr:href='data:post.url + &quot;#more&quot;' expr:title='data:post.title'><data:post.jumpText/></a>
      </div>
    </b:if>

    <div class='post-footer'>
    <div class='post-footer-line post-footer-line-1'>
      <span class='post-author vcard'>
        <b:if cond='data:top.showAuthor'>
          <data:top.authorLabel/>
            <b:if cond='data:post.authorProfileUrl'>
              <span class='fn' itemprop='author' itemscope='itemscope' itemtype='http://schema.org/Person'>
                <meta expr:content='data:post.authorProfileUrl' itemprop='url'/>
                <a class='g-profile' expr:href='data:post.authorProfileUrl' rel='author' title='author profile'>
                  <span itemprop='name'><data:post.author/></span>
                </a>
              </span>
            <b:else/>
              <span class='fn' itemprop='author' itemscope='itemscope' itemtype='http://schema.org/Person'>
                <span itemprop='name'><data:post.author/></span>
              </span>
            </b:if>
        </b:if>
      </span>

      <span class='post-timestamp'>
        <b:if cond='data:top.showTimestamp'>
          <data:top.timestampLabel/>
          <b:if cond='data:post.url'>
            <meta expr:content='data:post.url.canonical' itemprop='url'/>
            <a class='timestamp-link' expr:href='data:post.url' rel='bookmark' title='permanent link'><abbr class='published' expr:title='data:post.timestampISO8601' itemprop='datePublished'><data:post.timestamp/></abbr></a>
          </b:if>
        </b:if>
      </span>

      <span class='post-comment-link'>
        <b:include cond='data:blog.pageType not in {&quot;item&quot;,&quot;static_page&quot;}                          and data:post.allowComments' data='post' name='comment_count_picker'/>
      </span>

      <span class='post-icons'>
        <!-- email post links -->
        <b:if cond='data:post.emailPostUrl'>
          <span class='item-action'>
          <a expr:href='data:post.emailPostUrl' expr:title='data:top.emailPostMsg'>
            <img alt='' class='icon-action' height='13' src='https://resources.blogblog.com/img/icon18_email.gif' width='18'/>
          </a>
          </span>
        </b:if>

        <!-- quickedit pencil -->
        <b:include data='post' name='postQuickEdit'/>
      </span>

      <!-- share buttons -->
      <div class='post-share-buttons goog-inline-block'>
        <b:include cond='data:post.sharePostUrl' data='post' name='shareButtons'/>
      </div>

      </div>

      <div class='post-footer-line post-footer-line-2'>
      <span class='post-labels'>
        <b:if cond='data:top.showPostLabels and data:post.labels'>
          <data:postLabelsLabel/>
          <b:loop values='data:post.labels' var='label'>
            <a expr:href='data:label.url' rel='tag'><data:label.name/></a><b:if cond='not data:label.isLast'>,</b:if>
          </b:loop>
        </b:if>
      </span>
      </div>

      <div class='post-footer-line post-footer-line-3'>
      <span class='post-location'>
        <b:if cond='data:top.showLocation and data:post.location'>
          <data:postLocationLabel/>
          <a expr:href='data:post.location.mapsUrl' target='_blank'><data:post.location.name/></a>
        </b:if>
      </span>
      </div>
      <b:if cond='data:post.authorAboutMe'>
        <div class='author-profile' itemprop='author' itemscope='itemscope' itemtype='http://schema.org/Person'>
          <b:if cond='data:post.authorPhoto.url'>
            <img expr:src='data:post.authorPhoto.url' itemprop='image' width='50px'/>
          </b:if>
          <div>
            <a class='g-profile' expr:href='data:post.authorProfileUrl' itemprop='url' rel='author' title='author profile'>
              <span itemprop='name'><data:post.author/></span>
            </a>
          </div>
          <span itemprop='description'><data:post.authorAboutMe/></span>
        </div>
      </b:if>
    </div>
  </div>
</b:includable>
              <b:includable id='postQuickEdit' var='post'>
  <b:if cond='data:post.editUrl'>
    <span expr:class='&quot;item-control &quot; + data:post.adminClass'>
      <a expr:href='data:post.editUrl' expr:title='data:top.editPostMsg'>
        <img alt='' class='icon-action' height='18' src='https://resources.blogblog.com/img/icon18_edit_allbkg.gif' width='18'/>
      </a>
    </span>
  </b:if>
</b:includable>
              <b:includable id='shareButtons' var='post'>
  <b:if cond='data:top.showEmailButton'><a class='goog-inline-block share-button sb-email' expr:href='data:post.sharePostUrl + &quot;&amp;target=email&quot;' expr:title='data:top.emailThisMsg' target='_blank'><span class='share-button-link-text'><data:top.emailThisMsg/></span></a></b:if><b:if cond='data:top.showBlogThisButton'><a class='goog-inline-block share-button sb-blog' expr:href='data:post.sharePostUrl + &quot;&amp;target=blog&quot;' expr:onclick='&quot;window.open(this.href, \&quot;_blank\&quot;, \&quot;height=270,width=475\&quot;); return false;&quot;' expr:title='data:top.blogThisMsg' target='_blank'><span class='share-button-link-text'><data:top.blogThisMsg/></span></a></b:if><b:if cond='data:top.showTwitterButton'><a class='goog-inline-block share-button sb-twitter' expr:href='data:post.sharePostUrl + &quot;&amp;target=twitter&quot;' expr:title='data:top.shareToTwitterMsg' target='_blank'><span class='share-button-link-text'><data:top.shareToTwitterMsg/></span></a></b:if><b:if cond='data:top.showFacebookButton'><a class='goog-inline-block share-button sb-facebook' expr:href='data:post.sharePostUrl + &quot;&amp;target=facebook&quot;' expr:onclick='&quot;window.open(this.href, \&quot;_blank\&quot;, \&quot;height=430,width=640\&quot;); return false;&quot;' expr:title='data:top.shareToFacebookMsg' target='_blank'><span class='share-button-link-text'><data:top.shareToFacebookMsg/></span></a></b:if><b:if cond='data:top.showPinterestButton'><a class='goog-inline-block share-button sb-pinterest' expr:href='data:post.sharePostUrl + &quot;&amp;target=pinterest&quot;' expr:title='data:top.shareToPinterestMsg' target='_blank'><span class='share-button-link-text'><data:top.shareToPinterestMsg/></span></a></b:if>
</b:includable>
              <b:includable id='status-message'>
  <b:if cond='data:navMessage'>
  <div class='status-msg-wrap'>
    <div class='status-msg-body'>
      <data:navMessage/>
    </div>
    <div class='status-msg-border'>
      <div class='status-msg-bg'>
        <div class='status-msg-hidden'><data:navMessage/></div>
      </div>
    </div>
  </div>
  <div style='clear: both;'/>
  </b:if>
</b:includable>
              <b:includable id='threaded-comment-form' var='post'>
  <div class='comment-form'>
    <a name='comment-form'/>
    <b:if cond='data:mobile'>
      <p><data:blogCommentMessage/></p>
      <data:blogTeamBlogMessage/>
      <a expr:href='data:post.commentFormIframeSrc' id='comment-editor-src'/>
      <iframe allowtransparency='true' class='blogger-iframe-colorize blogger-comment-from-post' expr:height='data:cmtIframeInitialHeight' frameborder='0' id='comment-editor' name='comment-editor' src='' style='display: none' width='100%'/>
    <b:else/>
      <p><data:blogCommentMessage/></p>
      <data:blogTeamBlogMessage/>
      <a expr:href='data:post.commentFormIframeSrc' id='comment-editor-src'/>
      <iframe allowtransparency='true' class='blogger-iframe-colorize blogger-comment-from-post' expr:height='data:cmtIframeInitialHeight' frameborder='0' id='comment-editor' name='comment-editor' src='' width='100%'/>
    </b:if>
    <data:post.cmtfpIframe/>
    <script type='text/javascript'>
      BLOG_CMT_createIframe(&#39;<data:post.appRpcRelayPath/>&#39;);
    </script>
  </div>
</b:includable>
              <b:includable id='threaded_comment_js' var='post'>
  <script async='async' expr:src='data:post.commentSrc' type='text/javascript'/>

  <script type='text/javascript'>
    (function() {
      var items = <data:post.commentJso/>;
      var msgs = <data:post.commentMsgs/>;
      var config = <data:post.commentConfig/>;

// <![CDATA[
      var cursor = null;
      if (items && items.length > 0) {
        cursor = parseInt(items[items.length - 1].timestamp) + 1;
      }

      var bodyFromEntry = function(entry) {
        var text = (entry &&
                    ((entry.content && entry.content.$t) ||
                     (entry.summary && entry.summary.$t))) ||
            '';
        if (entry && entry.gd$extendedProperty) {
          for (var k in entry.gd$extendedProperty) {
            if (entry.gd$extendedProperty[k].name == 'blogger.contentRemoved') {
              return '<span class="deleted-comment">' + text + '</span>';
            }
          }
        }
        return text;
      }

      var parse = function(data) {
        cursor = null;
        var comments = [];
        if (data && data.feed && data.feed.entry) {
          for (var i = 0, entry; entry = data.feed.entry[i]; i++) {
            var comment = {};
            // comment ID, parsed out of the original id format
            var id = /blog-(\d+).post-(\d+)/.exec(entry.id.$t);
            comment.id = id ? id[2] : null;
            comment.body = bodyFromEntry(entry);
            comment.timestamp = Date.parse(entry.published.$t) + '';
            if (entry.author && entry.author.constructor === Array) {
              var auth = entry.author[0];
              if (auth) {
                comment.author = {
                  name: (auth.name ? auth.name.$t : undefined),
                  profileUrl: (auth.uri ? auth.uri.$t : undefined),
                  avatarUrl: (auth.gd$image ? auth.gd$image.src : undefined)
                };
              }
            }
            if (entry.link) {
              if (entry.link[2]) {
                comment.link = comment.permalink = entry.link[2].href;
              }
              if (entry.link[3]) {
                var pid = /.*comments\/default\/(\d+)\?.*/.exec(entry.link[3].href);
                if (pid && pid[1]) {
                  comment.parentId = pid[1];
                }
              }
            }
            comment.deleteclass = 'item-control blog-admin';
            if (entry.gd$extendedProperty) {
              for (var k in entry.gd$extendedProperty) {
                if (entry.gd$extendedProperty[k].name == 'blogger.itemClass') {
                  comment.deleteclass += ' ' + entry.gd$extendedProperty[k].value;
                } else if (entry.gd$extendedProperty[k].name == 'blogger.displayTime') {
                  comment.displayTime = entry.gd$extendedProperty[k].value;
                }
              }
            }
            comments.push(comment);
          }
        }
        return comments;
      };

      var paginator = function(callback) {
        if (hasMore()) {
          var url = config.feed + '?alt=json&v=2&orderby=published&reverse=false&max-results=50';
          if (cursor) {
            url += '&published-min=' + new Date(cursor).toISOString();
          }
          window.bloggercomments = function(data) {
            var parsed = parse(data);
            cursor = parsed.length < 50 ? null
                : parseInt(parsed[parsed.length - 1].timestamp) + 1
            callback(parsed);
            window.bloggercomments = null;
          }
          url += '&callback=bloggercomments';
          var script = document.createElement('script');
          script.type = 'text/javascript';
          script.src = url;
          document.getElementsByTagName('head')[0].appendChild(script);
        }
      };
      var hasMore = function() {
        return !!cursor;
      };
      var getMeta = function(key, comment) {
        if ('iswriter' == key) {
          var matches = !!comment.author
              && comment.author.name == config.authorName
              && comment.author.profileUrl == config.authorUrl;
          return matches ? 'true' : '';
        } else if ('deletelink' == key) {
          return config.baseUri + '/comment/delete/'
               + config.blogId + '/' + comment.id;
        } else if ('deleteclass' == key) {
          return comment.deleteclass;
        }
        return '';
      };

      var replybox = null;
      var replyUrlParts = null;
      var replyParent = undefined;

      var onReply = function(commentId, domId) {
        if (replybox == null) {
          // lazily cache replybox, and adjust to suit this style:
          replybox = document.getElementById('comment-editor');
          if (replybox != null) {
            replybox.height = '250px';
            replybox.style.display = 'block';
            replyUrlParts = replybox.src.split('#');
          }
        }
        if (replybox && (commentId !== replyParent)) {
          replybox.src = '';
          document.getElementById(domId).insertBefore(replybox, null);
          replybox.src = replyUrlParts[0]
              + (commentId ? '&parentID=' + commentId : '')
              + '#' + replyUrlParts[1];
          replyParent = commentId;
        }
      };

      var hash = (window.location.hash || '#').substring(1);
      var startThread, targetComment;
      if (/^comment-form_/.test(hash)) {
        startThread = hash.substring('comment-form_'.length);
      } else if (/^c[0-9]+$/.test(hash)) {
        targetComment = hash.substring(1);
      }

      // Configure commenting API:
      var configJso = {
        'maxDepth': config.maxThreadDepth
      };
      var provider = {
        'id': config.postId,
        'data': items,
        'loadNext': paginator,
        'hasMore': hasMore,
        'getMeta': getMeta,
        'onReply': onReply,
        'rendered': true,
        'initComment': targetComment,
        'initReplyThread': startThread,
        'config': configJso,
        'messages': msgs
      };

      var render = function() {
        if (window.goog && window.goog.comments) {
          var holder = document.getElementById('comment-holder');
          window.goog.comments.render(holder, provider);
        }
      };

      // render now, or queue to render when library loads:
      if (window.goog && window.goog.comments) {
        render();
      } else {
        window.goog = window.goog || {};
        window.goog.comments = window.goog.comments || {};
        window.goog.comments.loadQueue = window.goog.comments.loadQueue || [];
        window.goog.comments.loadQueue.push(render);
      }
    })();
// ]]>
  </script>
</b:includable>
              <b:includable id='threaded_comments' var='post'>
  <div class='comments' id='comments'>
    <a name='comments'/>
    <h4><data:post.commentLabelFull/>:</h4>

    <div class='comments-content'>
      <b:include cond='data:post.embedCommentForm' data='post' name='threaded_comment_js'/>
      <div id='comment-holder'>
         <data:post.commentHtml/>
      </div>
    </div>

    <p class='comment-footer'>
      <b:if cond='data:post.allowNewComments'>
        <b:include data='post' name='threaded-comment-form'/>
      <b:else/>
        <data:post.noNewCommentsText/>
      </b:if>
    </p>

    <b:if cond='data:showCmtPopup'>
      <div id='comment-popup'>
        <iframe allowtransparency='true' frameborder='0' id='comment-actions' name='comment-actions' scrolling='no'>
        </iframe>
      </div>
    </b:if>

    <div id='backlinks-container'>
    <div expr:id='data:widget.instanceId + &quot;_backlinks-container&quot;'>
    </div>
    </div>
  </div>
</b:includable>
            </b:widget>
            <b:widget id='FeaturedPost1' locked='false' title='' type='FeaturedPost'>
              <b:widget-settings>
                <b:widget-setting name='showSnippet'>true</b:widget-setting>
                <b:widget-setting name='showPostTitle'>false</b:widget-setting>
                <b:widget-setting name='postId'>0</b:widget-setting>
                <b:widget-setting name='showFirstImage'>true</b:widget-setting>
                <b:widget-setting name='useMostRecentPost'>true</b:widget-setting>
              </b:widget-settings>
              <b:includable id='main'>
  <!-- Only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <b:include name='content'/>

  <b:include name='quickedit'/>
</b:includable>
              <b:includable id='content'>
  <div class='post-summary'>
    <b:if cond='data:showPostTitle and data:postTitle != &quot;&quot;'>
      <h3><a expr:href='data:postUrl'><data:postTitle/></a></h3>
    </b:if>
    <b:if cond='data:showSnippet and data:postSummary != &quot;&quot;'>
      <p>
        <data:postSummary/>
      </p>
    </b:if>
    <b:if cond='data:showFirstImage and data:postFirstImage != &quot;&quot;'>
      <img class='image' expr:src='data:postFirstImage'/>
    </b:if>
  </div>

  <style type='text/css'>
    .image {
      width: 100%;
    }
  </style>
</b:includable>
            </b:widget>
            <b:widget id='PopularPosts1' locked='false' title='' type='PopularPosts'>
              <b:widget-settings>
                <b:widget-setting name='numItemsToShow'>5</b:widget-setting>
                <b:widget-setting name='showThumbnails'>false</b:widget-setting>
                <b:widget-setting name='showSnippets'>true</b:widget-setting>
                <b:widget-setting name='timeRange'>ALL_TIME</b:widget-setting>
              </b:widget-settings>
              <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'><h2><data:title/></h2></b:if>
  <div class='widget-content popular-posts'>
    <ul>
      <b:loop values='data:posts' var='post'>
      <li>
        <b:if cond='!data:showThumbnails'>
          <b:if cond='!data:showSnippets'>
            <!-- (1) No snippet/thumbnail -->
            <a expr:href='data:post.href'><data:post.title/></a>
          <b:else/>
            <!-- (2) Show only snippets -->
            <div class='item-title'><a expr:href='data:post.href'><data:post.title/></a></div>
            <div class='item-snippet'><data:post.snippet/></div>
          </b:if>
        <b:else/>
          <!-- (3) Show only thumbnails or (4) Snippets and thumbnails. -->
          <div expr:class='data:showSnippets ? &quot;item-content&quot; : &quot;item-thumbnail-only&quot;'>
            <b:if cond='data:post.featuredImage.isResizable or data:post.thumbnail'>
              <div class='item-thumbnail'>
                <a expr:href='data:post.href' target='_blank'>
                  <b:with value='data:post.featuredImage.isResizable                                  ? resizeImage(data:post.featuredImage, 72, &quot;1:1&quot;)                                  : data:post.thumbnail' var='image'>
                    <img alt='' border='0' expr:src='data:image'/>
                  </b:with>
                </a>
              </div>
            </b:if>
            <div class='item-title'><a expr:href='data:post.href'><data:post.title/></a></div>
            <b:if cond='data:showSnippets'>
              <div class='item-snippet'><data:post.snippet/></div>
            </b:if>
          </div>
          <div style='clear: both;'/>
        </b:if>
      </li>
      </b:loop>
    </ul>
    <b:include name='quickedit'/>
  </div>
</b:includable>
            </b:widget>
          </b:section>
        </div>
        </div>

        <div class='column-left-outer'>
        <div class='column-left-inner'>
          <aside>
          <macro:include id='main-column-left-sections' name='sections'>
            <macro:param default='0' name='num' value='0'/>
            <macro:param default='sidebar-left' name='idPrefix'/>
            <macro:param default='sidebar' name='class'/>
            <macro:param default='true' name='includeBottom'/>
          </macro:include>
          </aside>
        </div>
        </div>

        <div class='column-right-outer'>
        <div class='column-right-inner'>
          <aside>
          <macro:include id='main-column-right-sections' name='sections'>
            <macro:param default='2' name='num' value='0'/>
            <macro:param default='sidebar-right' name='idPrefix'/>
            <macro:param default='sidebar' name='class'/>
            <macro:param default='true' name='includeBottom'/>
          </macro:include>
          </aside>
        </div>
        </div>

        </div>

        <div style='clear: both'/>
      <!-- columns -->
      </div>

    <!-- main -->
    </div>
    </div>
    <div class='main-cap-bottom cap-bottom'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    </div>

    <footer>
    <div class='footer-outer'>
    <div class='footer-cap-top cap-top'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    <div class='fauxborder-left footer-fauxborder-left'>
    <div class='fauxborder-right footer-fauxborder-right'/>
    <div class='region-inner footer-inner'>
      <macro:include id='footer-sections' name='sections'>
        <macro:param default='2' name='num' value='3'/>
        <macro:param default='footer' name='idPrefix'/>
        <macro:param default='foot' name='class'/>
        <macro:param default='false' name='includeBottom'/>
      </macro:include>
      <!-- outside of the include in order to lock Attribution widget -->
      <b:section class='foot' id='footer-3' name='Footer' showaddelement='no'>
        <b:widget id='Attribution1' locked='false' title='' type='Attribution'>
          <b:widget-settings>
            <b:widget-setting name='copyright'><![CDATA[© 2025 Aleo&#39;s Tube. All Rights Reserved.]]></b:widget-setting>
          </b:widget-settings>
          <b:includable id='main'>
    <div class='widget-content' style='text-align: center;'>
      <b:if cond='data:attribution != &quot;&quot;'>
       <data:attribution/>
      </b:if>
    </div>

    <b:include name='quickedit'/>
  </b:includable>
        </b:widget>
      </b:section>
    </div>
    </div>
    <div class='footer-cap-bottom cap-bottom'>
      <div class='cap-left'/>
      <div class='cap-right'/>
    </div>
    </div>
    </footer>

  <!-- content -->
  </div>
  </div>
  <div class='content-cap-bottom cap-bottom'>
    <div class='cap-left'/>
    <div class='cap-right'/>
  </div>
  </div>
  </div>

  <script type='text/javascript'>
    window.setTimeout(function() {
        document.body.className = document.body.className.replace(&#39;loading&#39;, &#39;&#39;);
      }, 10);
  </script><!-- Google Tag Manager (noscript) -->
<noscript>
  <iframe height='0' src='https://www.googletagmanager.com/ns.html?id=GTM-TRBCNSHZ&amp;env=1&amp;auth=XlVCJiNF15b9I-YIWjI4Dg' style='display:none;visibility:hidden' width='0'/>
</noscript>
<!-- End Google Tag Manager (noscript) -->
<!-- Google Tag Manager (noscript) -->
<noscript><iframe height='0' src='https://www.googletagmanager.com/ns.html?id=GTM-WR546M4D' style='display:none;visibility:hidden' width='0'/></noscript>
<!-- End Google Tag Manager (noscript) -->
<!-- Google Tag Manager (noscript) -->
<noscript>
  <iframe height='0' src='https://www.googletagmanager.com/ns.html?id=GTM-MKB9TPDR' style='display:none;visibility:hidden' width='0'/>
</noscript>
<!-- End Google Tag Manager (noscript) -->
<!-- =================================================== -->
<!-- 🤖 ALEO&#8217;S TUBE SEO MAX v10.3 Ultimate (No-ID ProFix) -->
<!-- =================================================== -->
<b:if cond='data:blog.pageType'>
  <![CDATA[

  <!-- Google Analytics -->
  <script async="async" src="https://www.googletagmanager.com/gtag/js?id=G-2F6SD6VRNM"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config','G-2F6SD6VRNM');
  </script>

  <!-- Consent + Auto Ping -->
  <script>
  (function(){
    const consentKey='gt_accept_v10_3';
    const pingKey='gt_ping_v10_3';
    const sitemap=location.origin+'/sitemap.xml';

    function runPing(){
      try{
        const now=Date.now();
        const last=localStorage.getItem(pingKey);
        if(!last||(now-parseInt(last,10)>86400000)){
          fetch('https://www.google.com/ping?sitemap='+encodeURIComponent(sitemap),{mode:'no-cors'});
          fetch('https://www.bing.com/ping?sitemap='+encodeURIComponent(sitemap),{mode:'no-cors'});
          localStorage.setItem(pingKey,String(now));
          console.log('[SEO MAX v10.3] Ping sent.');
        }
      }catch(e){console.warn('Ping error',e);}
    }

    if(!localStorage.getItem(consentKey)){
      const box=document.createElement('div');
      box.id='seoConsent';
      box.innerHTML='<div style="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,.9);color:#fff;padding:18px 26px;border-radius:18px;box-shadow:0 0 24px rgba(0,170,255,.6);font-family:Segoe UI,Roboto,sans-serif;z-index:9999;text-align:center;backdrop-filter:blur(8px)"><b>Aktifkan SEO MAX v10.3</b><br><small>Izinkan analytics &amp; ping otomatis</small><br><button id="seoAccept" style="margin-top:10px;padding:6px 14px;border:none;border-radius:8px;background:linear-gradient(90deg,#ff6b00,#0077ff);color:#fff;font-weight:700;cursor:pointer">Terima</button></div>';
      document.body.appendChild(box);
      document.getElementById('seoAccept').addEventListener('click',()=>{
        localStorage.setItem(consentKey,'1');
        document.getElementById('seoConsent').remove();
        runPing();
      });
    }else{ runPing(); }
  })();
  </script>

  <!-- Schema JSON-LD (Website + Organization) -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@graph": [
      {
        "@type": "WebSite",
        "@id": "https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/#website",
        "url": "https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/",
        "name": "Aleo’s Tube Daily Vlog Anak Rantau",
        "description": "Kisah, pengalaman, dan keseharian anak rantau – Aleo’s Tube Daily Vlog.",
        "publisher": { "@id": "#organization" },
        "inLanguage": "id-ID",
        "potentialAction": {
          "@type": "SearchAction",
          "target": "https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/search?q={search_term_string}",
          "query-input": "required name=search_term_string"
        }
      },
      {
        "@type": "Organization",
        "@id": "#organization",
        "name": "Aleo’s Tube",
        "url": "https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/",
        "logo": {
          "@type": "ImageObject",
          "url": "https://blogger.googleusercontent.com/img/b/logo.png",
          "width": 512,
          "height": 512
        },
        "sameAs": [
          "https://www.youtube.com/channel/UCTHGlP7T12oHv2QHDuw0C3g",
          "https://www.instagram.com/aleos_tube/",
          "https://www.facebook.com/aleostube"
        ]
      }
    ]
  }
  </script>

  <!-- Schema JSON-LD untuk BlogPosting -->
  <b:if cond='data:blog.pageType == &quot;item&quot;'>
    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "BlogPosting",
      "mainEntityOfPage": {
        "@type": "WebPage",
        "@id": "<data:post.url/>"
      },
      "headline": "<data:post.title.escaped/>",
      "description": "<data:post.snippet/>",
      "image": {
        "@type": "ImageObject",
        "url": "<data:post.firstImageUrl/>"
      },
      "author": {
        "@type": "Person",
        "name": "<data:post.author/>",
        "url": "https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/"
      },
      "publisher": {
        "@type": "Organization",
        "name": "Aleo’s Tube",
        "logo": {
          "@type": "ImageObject",
          "url": "https://blogger.googleusercontent.com/img/b/logo.png"
        }
      },
      "datePublished": "<data:post.date.iso8601/>",
      "dateModified": "<data:post.dateLastUpdated.iso8601/>",
      "inLanguage": "id-ID"
    }
    </script>
  </b:if>

  <style>
    @media(prefers-color-scheme:dark){
      body{background:#0b0d12;color:#eee;}
    }
  </style>

  ]]>
</b:if>
<!-- =================================================== -->
<!-- &#9989; END SEO MAX v10.3 Ultimate (No-ID ProFix) -->
<!-- =================================================== -->
<!-- 💎 ALEO&#8217;S TUBE &#8212; SMART AUTO-LABEL SCROLLER v10.0 LUXE FADE EDITION -->
<div id='aleos-grid-aff'/>

<script type='text/javascript'>
//<![CDATA[
(async function(){
  const BLOG_URL = 'https://aleos-tube-daily-vlog-anak-rantau.blogspot.com';

  // 🧭 Deteksi label dari URL
  let currentLabel = (location.pathname.match(/\/label\/([^/?#]+)/i)||[])[1];
  if(currentLabel) currentLabel = decodeURIComponent(currentLabel);
  const LABEL = currentLabel || 'Afiliasi';
  const FEED_URL = `${BLOG_URL}/feeds/posts/default/-/${LABEL}?alt=json-in-script&max-results=50`;

  // 🎨 Ikon & warna label
  const STYLEMAP = {
    'afiliasi': {icon:'💰', grad:'linear-gradient(135deg,#ffedd5,#fff7ed)'},
    'bisnis': {icon:'💼', grad:'linear-gradient(135deg,#dbeafe,#eff6ff)'},
    'vlog': {icon:'🎥', grad:'linear-gradient(135deg,#fee2e2,#fef2f2)'},
    'hiburan': {icon:'🎬', grad:'linear-gradient(135deg,#dcfce7,#f0fdf4)'},
    'rantau': {icon:'✈️', grad:'linear-gradient(135deg,#cffafe,#ecfeff)'},
    'produk': {icon:'🛒', grad:'linear-gradient(135deg,#fef9c3,#fefce8)'},
    'inspirasi': {icon:'🌟', grad:'linear-gradient(135deg,#ede9fe,#f5f3ff)'},
    'default': {icon:'🔥', grad:'linear-gradient(135deg,#f3f4f6,#f9fafb)'}
  };

  const THEME = STYLEMAP[LABEL.toLowerCase()] || STYLEMAP['default'];

  // 🧩 Struktur utama
  const wrap = document.getElementById('aleos-grid-aff');
  wrap.innerHTML = `
  <style>
    #aleos-grid-aff {
      font-family: Inter, system-ui;
      background: ${THEME.grad};
      padding: 16px;
      border-radius: 18px;
      margin: 18px 0;
      box-shadow: 0 4px 14px rgba(0,0,0,.08);
      overflow: hidden;
      color: #0f172a;
      opacity: 0;
      transform: translateY(10px);
      animation: fadeInUp .9s ease-out forwards;
    }

    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @media (prefers-color-scheme: dark) {
      #aleos-grid-aff {
        background: linear-gradient(135deg,#0b1325,#111b2f);
        color: #e6eef8;
      }
    }

    #aleos-grid-aff h3 {
      margin: 0 0 10px;
      font-size: 1.05rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .scroll-box {
      display: flex;
      overflow-x: auto;
      gap: 14px;
      padding-bottom: 6px;
      scroll-behavior: smooth;
      scrollbar-width: thin;
      animation: fadeIn .8s ease-out forwards;
    }

    @keyframes fadeIn { from { opacity: 0 } to { opacity: 1 } }

    .scroll-box::-webkit-scrollbar { height: 6px; }
    .scroll-box::-webkit-scrollbar-thumb {
      background: rgba(0,0,0,.2);
      border-radius: 4px;
    }

    .ao-item {
      flex: 0 0 180px;
      background: #fff;
      border-radius: 14px;
      overflow: hidden;
      box-shadow: 0 4px 10px rgba(0,0,0,.06);
      transition: transform .25s ease, box-shadow .25s;
      opacity: 0;
      animation: fadeInCard .6s ease forwards;
    }

    @keyframes fadeInCard {
      from { opacity: 0; transform: scale(.97); }
      to { opacity: 1; transform: scale(1); }
    }

    .ao-item:hover {
      transform: translateY(-4px);
      box-shadow: 0 6px 14px rgba(0,0,0,.1);
    }

    .ao-thumb {
      width: 100%;
      aspect-ratio: 1/1;
      object-fit: cover;
      display: block;
    }

    .ao-title {
      font-size: 13px;
      font-weight: 600;
      margin: 8px 10px;
      color: #111827;
      line-height: 1.3;
      min-height: 36px;
    }

    .ao-cta {
      margin: 0 10px 12px;
      padding: 7px;
      text-align: center;
      border-radius: 8px;
      color: #fff;
      text-decoration: none;
      font-weight: 700;
      display: block;
      transition: opacity .2s;
    }
    .ao-cta:hover { opacity: 0.85; }

    .blue { background: #2563eb; }
    .orange { background: #f97316; }
    .red { background: #ef4444; }

    @media (max-width: 480px) {
      .ao-item { flex: 0 0 150px; }
      h3 { font-size: .95rem; }
    }
  </style>

  <h3 id="labelTitle">${THEME.icon} Memuat...</h3>
  <div class="scroll-box" id="scroll-box"></div>
  `;

  const grid = document.getElementById('scroll-box');
  const titleEl = document.getElementById('labelTitle');
  titleEl.textContent = `${THEME.icon} ${LABEL} Terbaru dari Aleo’s Tube`;

  const FALLBACK = [
    {title:"Aleo's Tube — Premium Business Program", img:"https://www.digistore24-app.com/pb/img/merchant_371937/image/product/UOW66CMM.jpg", href:"https://www.digistore24.com/redir/472943/Aleostube/"},
    {title:"InGenius Wave — Audio Focus", img:"https://www.digistore24-app.com/pb/img/merchant_3646207/image/product/A6KAKV3T.png", href:"https://ingeniuswave.com/DSvsl/#aff=Aleostube"},
    {title:"Pineal10X — Mindset", img:"https://www.digistore24-app.com/pb/img/merchant_1632947/image/product/CWSY9T6E.png", href:"https://pineal10x.com/pnl10-index.php#aff=Aleostube"},
    {title:"Superhuman At 70 — Supplement", img:"https://www.digistore24-app.com/pb/img/merchant_2005677/image/product/H4VEJAAT.gif", href:"https://www.advancedbionutritionals.com/DS24/Nitric-Oxide-Supplements/Superhuman-At-70/HD.htm#aff=Aleostube"},
    {title:"Meridian Acupressure Mat", img:"https://www.digistore24-app.com/pb/img/merchant_771978/image/product/2WY9BT3X.png", href:"https://andbalanced.com/pages/meridian-acupressure-mat-and-pillow-set-dg#aff=Aleostube"}
  ];

  function makeNode(item,i){
    const color=['blue','orange','red'][i%3];
    const el=document.createElement('div');
    el.className='ao-item';
    el.innerHTML=`
      <img class="ao-thumb" loading="lazy" src="${item.img}" alt="${item.title}">
      <div class="ao-title">${item.title}</div>
      <a class="ao-cta ${color}" href="${item.href}" target="_blank" rel="nofollow noopener">Lihat</a>
    `;
    el.style.animationDelay = (i * 0.12) + 's';
    return el;
  }

  async function loadData(){
    grid.innerHTML='';
    try{
      const res=await fetch(FEED_URL,{cache:'no-store'});
      const txt=await res.text();
      const json=JSON.parse(txt.slice(txt.indexOf('{'),txt.lastIndexOf('}')+1));
      const entries=json.feed.entry||[];
      if(entries.length){
        entries.forEach((p,i)=>{
          const title=p.title?.$t||"Tanpa Judul";
          const href=(p.link?.find(l=>l.rel==="alternate")||{}).href||BLOG_URL;
          let img=p.media$thumbnail?.url;
          if(!img&&p.content?.$t){
            const m=p.content.$t.match(/<img[^>]+src="([^">]+)/i);
            if(m) img=m[1];
          }
          img=img||"https://via.placeholder.com/400x400?text=Aleo+Offer";
          grid.appendChild(makeNode({title,img,href},i));
        });
      } else FALLBACK.forEach((f,i)=>grid.appendChild(makeNode(f,i)));
    }catch(e){
      FALLBACK.forEach((f,i)=>grid.appendChild(makeNode(f,i)));
    }
  }

  // 🌈 Auto-scroll lembut
  function autoScroll(el){
    let speed = 0.5;
    let pause = false;
    function step(){
      if(!pause){
        el.scrollLeft += speed;
        if(el.scrollLeft >= el.scrollWidth - el.clientWidth){ el.scrollLeft = 0; }
      }
      requestAnimationFrame(step);
    }
    el.addEventListener('mouseenter', ()=>pause = true);
    el.addEventListener('mouseleave', ()=>pause = false);
    step();
  }

  await loadData();
  autoScroll(grid);
  setInterval(loadData, 60000);
})();
//]]>
</script>
<!-- 🧩 END SMART AUTO-LABEL SCROLLER v10.0 LUXE FADE EDITION -->
<!-- 🌸 Smart Accessibility Widget v10.2 &#8211; Baby Voice Blogger Safe -->
<style>
  :root {
    --widget-bg: #ffffff;
    --widget-text: #000000;
    --widget-accent: #ff66cc;
    --widget-shadow: rgba(0,0,0,0.3);
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --widget-bg: #121212;
      --widget-text: #ffffff;
      --widget-accent: #ff99dd;
      --widget-shadow: rgba(255,255,255,0.2);
    }
  }

  #virtualsign-widget {
    position: fixed;
    bottom: 100px;
    right: 200px;
    z-index: 9999;
    width: 320px;
    height: 240px;
    box-shadow: 0 0 12px var(--widget-shadow);
    border-radius: 15px;
    overflow: hidden;
    opacity: 0;
    visibility: hidden;
    transition: all 0.5s ease;
    background: var(--widget-bg);
  }
  #virtualsign-widget.show {opacity: 1; visibility: visible;}

  .access-btn {
    position: fixed;
    bottom: 30px;
    z-index: 10000;
    border: none;
    border-radius: 50%;
    width: 60px;
    height: 60px;
    font-size: 28px;
    cursor: pointer;
    color: #fff;
    box-shadow: 0 0 10px var(--widget-shadow);
    transition: transform 0.3s ease, background 0.3s ease;
  }
  .access-btn:hover {transform: scale(1.1);}
  #toggle-sign { right: 200px; background: var(--widget-accent); }
  #toggle-voice { right: 275px; background: #00cc99; }

  #sign-label {
    position: fixed;
    bottom: 95px;
    right: 200px;
    background: rgba(0,0,0,0.8);
    color: #fff;
    padding: 6px 12px;
    border-radius: 10px;
    font-size: 15px;
    font-weight: 500;
    opacity: 0;
    visibility: hidden;
    z-index: 10001;
    transition: opacity 0.4s ease;
  }
  #sign-label.show {opacity: 1; visibility: visible;}

  #voice-select {
    position: fixed;
    bottom: 100px;
    right: 340px;
    z-index: 10002;
    background: var(--widget-bg);
    color: var(--widget-text);
    border-radius: 10px;
    border: 1px solid #ccc;
    font-size: 14px;
    padding: 6px 10px;
    display: none;
  }
  #voice-select.show {display: block;}

  @media (max-width: 768px) {
    #virtualsign-widget { width: 260px; height: 200px; bottom: 90px; left: 20px; right: auto; }
    #toggle-sign { left: 20px; right: auto; }
    #toggle-voice { left: 95px; right: auto; }
    #sign-label { left: 20px; right: auto; }
    #voice-select { left: 160px; right: auto; }
  }
</style>

<div id='virtualsign-widget'>
  <iframe allowfullscreen='true' frameborder='0' height='100%' id='sign-iframe' src='https://www.virtualsign.net/embed' title='Virtual Sign Language Widget' width='100%'/>
</div>

<div id='sign-label'>Bahasa Isyarat Aktif 🤟</div>
<button class='access-btn' id='toggle-sign' title='Bahasa Isyarat'>🤟</button>
<button class='access-btn' id='toggle-voice' title='Dengar Suara'>🍼</button>
<select id='voice-select' title='Pilih Bahasa Suara'>
  <option value='id-ID'>🇮🇩 Indonesia</option>
  <option value='en-US'>🇬🇧 English</option>
  <option value='es-ES'>🇪🇸 Español</option>
  <option value='ja-JP'>🇯🇵 日本語</option>
  <option value='ar-SA'>🇸🇦 العربية</option>
</select>

<script type='text/javascript'>
//<![CDATA[
(function(){
  var synth = window.speechSynthesis;
  var widget = document.getElementById('virtualsign-widget');
  var toggleSign = document.getElementById('toggle-sign');
  var toggleVoice = document.getElementById('toggle-voice');
  var label = document.getElementById('sign-label');
  var iframe = document.getElementById('sign-iframe');
  var voiceSelect = document.getElementById('voice-select');
  var visible = false;
  var currentLang = navigator.language || 'id-ID';

  var greetings = {
    'id': "Haii~ aku asisten kecilmu 🍼 aku siap bantu kamu baca atau aku bacain yaa~!",
    'en': "Hewwo~ I'm your tiny helper! 🍼 I can read for you or help you listen~",
    'es': "¡Hola pequeñito! 🍼 Soy tu ayudante y puedo leer o hablar contigo~",
    'ja': "やっほ〜！🍼 ぼくはあなたのちっちゃなアシスタントだよ！読むのも聞くのもお手伝いするね〜！",
    'ar': "مرحبًا يا صديقي الصغير 🍼 أنا مساعدك الصغير، أقرأ لك أو أتكلم معك بلطف~"
  };

  function speakText(text, lang){
    if('speechSynthesis' in window){
      synth.cancel();
      var utter = new SpeechSynthesisUtterance(text);
      utter.lang = lang;
      utter.rate = 1.1;
      utter.pitch = 1.6;
      synth.speak(utter);
    }
  }

  window.addEventListener('load', function(){
    var langCode = currentLang.split('-')[0];
    var greeting = greetings[langCode] || greetings['en'];
    setTimeout(function(){ speakText(greeting, currentLang); }, 1200);
  });

  toggleSign.addEventListener('click', function(){
    visible = !visible;
    widget.classList.toggle('show', visible);
    label.classList.toggle('show', visible);
    voiceSelect.classList.toggle('show', visible);
  });

  voiceSelect.addEventListener('change', function(){
    currentLang = this.value;
  });

  toggleVoice.addEventListener('click', function(){
    var selectedText = window.getSelection().toString().trim();
    if(selectedText.length < 2){
      speakText("Ehh... kamu lupa pilih teksnya dulu~ 🍼", currentLang);
      return;
    }
    speakText(selectedText, currentLang);
  });

  document.addEventListener('mouseup', function(){
    var selectedText = window.getSelection().toString().trim();
    if(selectedText.length > 2){
      visible = true;
      widget.classList.add('show');
      label.classList.add('show');
      voiceSelect.classList.add('show');
      iframe.src = 'https://www.virtualsign.net/embed?text=' + encodeURIComponent(selectedText);
      speakText(selectedText, currentLang);
    }
  });
})();
//]]>
</script>
<!-- 🌸 End Smart Accessibility Widget v10.2 -->

<!-- =========================================================
🚀 ALEO&#8217;S TUBE SEO MAX v10.1 PRO
SMART PING PROTECTOR + AUTO INDEXNOW KEY GENERATOR
========================================================= -->
<script>
(function(){
  const blogURL = &quot;https://aleos-tube-daily-vlog-anak-rantau.blogspot.com&quot;;
  const canonical = document.querySelector(&#39;link[rel=&quot;canonical&quot;]&#39;);
  const pageURL = canonical ? canonical.href : window.location.href;
  const isPostPage = /\/20\d{2}\//.test(pageURL);

  // 🔑 Key generator (gunakan nilai acak sebagai key sementara)
  const defaultKey = &quot;aleostube-&quot; + Math.random().toString(36).substring(2,10);

  // Simpan tanggal terakhir ping
  const today = new Date().toISOString().split(&quot;T&quot;)[0];
  const lastPingKey = &quot;AleoPing_&quot; + pageURL;
  const lastPingDate = localStorage.getItem(lastPingKey);

  if (isPostPage &amp; lastPingDate !== today) {
    const pingURLs = [
      &quot;https://www.google.com/ping?sitemap=&quot; + encodeURIComponent(blogURL + &quot;/sitemap.xml&quot;),
      &quot;https://www.bing.com/ping?sitemap=&quot; + encodeURIComponent(blogURL + &quot;/sitemap.xml&quot;),
      &quot;https://yandex.com/ping?sitemap=&quot; + encodeURIComponent(blogURL + &quot;/sitemap.xml&quot;),
      &quot;https://api.indexnow.org/indexnow?url=&quot; + encodeURIComponent(pageURL) + &quot;&amp;key=&quot; + defaultKey,
      &quot;https://duckduckgo.com/ping?sitemap=&quot; + encodeURIComponent(blogURL + &quot;/sitemap.xml&quot;)
    ];

    console.groupCollapsed(&quot;🌐 ALEO&#8217;S TUBE SEO MAX v10.1 PRO &#8212; Auto Ping Log&quot;);
    pingURLs.forEach(url=&gt;{
      fetch(url,{method:&quot;GET&quot;,mode:&quot;no-cors&quot;})
        .then(()=&gt;console.log(&quot;&#9989; Ping terkirim ke:&quot;,url))
        .catch(err=&gt;console.warn(&quot;&#9888;&#65039; Gagal ping:&quot;,url,err));
    });
    console.groupEnd();

    localStorage.setItem(lastPingKey, today);
  } else {
    console.log(&quot;🛡&#65039; Ping sudah dilakukan hari ini untuk:&quot;, pageURL);
  }
})();
</script>

<!-- =========================================================
💡 PETUNJUK:
1&#65039;&#8419; Tempel di bawah </body> template Blogger.
2&#65039;&#8419; Pastikan robots.txt & meta SEO v9.9 aktif.
3&#65039;&#8419; Ping otomatis 1&#215; per artikel per hari (anti-spam).
4&#65039;&#8419; Uji di console browser (Ctrl+Shift+I &#8594; Console).
5&#65039;&#8419; Bisa ubah 'defaultKey' ke key resmi IndexNow milikmu:
    👉 https://www.bing.com/indexnow/getstarted
========================================================= -->

</body>

<macro:includable id='sections' var='col'>
  <macro:if cond='data:col.num == 0'>
  <macro:else/>
    <b:section mexpr:class='data:col.class' mexpr:id='data:col.idPrefix + &quot;-1&quot;' preferred='yes' showaddelement='yes'/>

    <macro:if cond='data:col.num &gt;= 2'>
      <table border='0' cellpadding='0' cellspacing='0' mexpr:class='&quot;section-columns columns-&quot; + data:col.num'>
      <tbody>
      <tr>
        <td class='first columns-cell'>
          <b:section mexpr:class='data:col.class' mexpr:id='data:col.idPrefix + &quot;-2-1&quot;'/>
        </td>

        <td class='columns-cell'>
          <b:section mexpr:class='data:col.class' mexpr:id='data:col.idPrefix + &quot;-2-2&quot;'/>
        </td>

        <macro:if cond='data:col.num &gt;= 3'>
          <td class='columns-cell'>
            <b:section mexpr:class='data:col.class' mexpr:id='data:col.idPrefix + &quot;-2-3&quot;'/>
          </td>
        </macro:if>

        <macro:if cond='data:col.num &gt;= 4'>
          <td class='columns-cell'>
            <b:section mexpr:class='data:col.class' mexpr:id='data:col.idPrefix + &quot;-2-4&quot;'/>
          </td>
        </macro:if>
      </tr>
      </tbody>
      </table>

      <macro:if cond='data:col.includeBottom'>
        <b:section mexpr:class='data:col.class' mexpr:id='data:col.idPrefix + &quot;-3&quot;' showaddelement='no'/>
      </macro:if>
    </macro:if>
  </macro:if>
</macro:includable>

<b:section-contents id='footer-1'>
  <b:widget id='HTML18' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- 💠 ALEOSTUBE FLEX HEADER v10 — Premium Responsive Edition -->
<style>
/* ============================================================
   💠 ALEOTUBE FLEX HEADER v10 — RESPONSIVE PREMIUM EDITION
   ============================================================ */

.header-outer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  box-sizing: border-box;
  text-align: center;
  position: relative;
  overflow: hidden;
  background: rgba(255,255,255,0.08);
  backdrop-filter: blur(10px);
  border-bottom: 2px solid rgba(255,255,255,0.1);
  padding: 1.5rem 1rem;
}
.header-outer::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg,
    rgba(255,140,0,0.3) 0%,
    rgba(0,123,255,0.25) 35%,
    rgba(255,0,0,0.25) 70%,
    rgba(255,255,255,0.1) 100%);
  z-index: 0;
}
.Header img {
  position: relative;
  z-index: 1;
  max-width: 180px;
  height: auto;
  border-radius: 12px;
  margin-bottom: 0.5rem;
  transition: transform 0.4s ease;
}
.Header img:hover { transform: scale(1.05); }
.Header h1 {
  position: relative;
  z-index: 1;
  font-size: clamp(1.8rem, 4vw, 2.8rem);
  color: #fff;
  text-shadow: 1px 1px 8px rgba(0,0,0,0.4);
  margin: 0.3rem 0;
  font-weight: 700;
  letter-spacing: 1px;
}
.Header .description {
  position: relative;
  z-index: 1;
  font-size: clamp(1rem, 2.5vw, 1.2rem);
  color: rgba(255,255,255,0.85);
  margin-top: 0.2rem;
  font-weight: 400;
}

/* Tabs / Menu */
.tabs-inner {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  width: 100%;
  gap: 8px;
  margin: 1rem auto 0.5rem;
  padding: 0.5rem 0;
  position: relative;
  z-index: 2;
}
.tabs-inner ul {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 6px;
}
.tabs-inner li a {
  display: inline-block;
  padding: 0.5rem 1rem;
  border-radius: 9999px;
  background: rgba(255,255,255,0.15);
  color: #fff;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.95rem;
  letter-spacing: 0.5px;
  transition: all 0.3s ease;
  border: 1px solid rgba(255,255,255,0.15);
}
.tabs-inner li a:hover,
.tabs-inner li.active a {
  background: linear-gradient(90deg, #ff7b00, #007bff, #ff003c);
  background-size: 200% 200%;
  animation: gradientMove 2s ease infinite;
  color: #fff;
  transform: translateY(-2px);
  border-color: transparent;
}
@keyframes gradientMove {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

/* Responsive */
@media (max-width: 768px) {
  .Header img { max-width: 150px; }
  .Header h1 { font-size: 1.9rem; }
  .Header .description { font-size: 1rem; }
  .tabs-inner li a { padding: 0.45rem 0.9rem; font-size: 0.9rem; }
}
@media (max-width: 480px) {
  .header-outer { padding: 1rem 0.5rem; }
  .Header img { max-width: 130px; }
  .Header h1 { font-size: 1.6rem; }
  .tabs-inner { flex-direction: column; gap: 5px; }
  .tabs-inner li a { width: 100%; text-align: center; }
}
</style>

<!-- ===========================
     HEADER STRUCTURE
     =========================== -->
<div class="header-outer">
  <div class="Header">
    <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj_LOGO-SAMPLE.png" alt="Aleo's Tube Logo" />
    <h1>Aleo’s Tube Daily Vlog Anak Rantau</h1>
    <p class="description">Berbagi kisah, pengalaman, dan keseharian penuh warna 🌏</p>
  </div>

  <div class="tabs-inner">
    <ul>
      <li class="active"><a href="/">Beranda</a></li>
      <li><a href="/p/about.html">Tentang</a></li>
      <li><a href="/p/afiliasi.html">Afiliasi</a></li>
      <li><a href="/p/kontak.html">Kontak</a></li>
      <li><a href="/search/label/Vlog">Vlog</a></li>
    </ul>
  </div>
</div>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='HTML5' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- 🌍 Multilingual Sitemap Generator - Aleo's Tube -->
<script>
(function() {
  const base = "https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/";
  const langs = ['id','en','tet','pt','es','fr','de','ja','zh','ko','tl','ms','vi','th','ar','ru'];
  const pages = [
    '', 'about', 'contact', 'produk', 'travel', 'vlog', 'inspirasi', 'tips-hidup-mandiri', 'promo', 'review'
  ];

  // Buat sitemap XML
  let xml = '<?xml version="1.0" encoding="UTF-8"?>\n<urlset>xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">\n';
  langs.forEach(lang => {
    pages.forEach(page => {
      xml += `  <url>\n`;
      xml += `    <loc>${base}${lang === 'id' ? '' : lang + '/'}${page}</loc>\n`;
      xml += `    <changefreq>weekly</changefreq>\n`;
      xml += `    <priority>0.8</priority>\n`;
      xml += `  </url>\n`;
    });
  });
  xml += '</urlset>';

  // Buat sitemap JSON
  const json = {
    site: base,
    updated: new Date().toISOString(),
    languages: langs,
    urls: langs.map(lang => ({
      lang,
      pages: pages.map(p => base + (lang === 'id' ? '' : lang + '/') + p)
    }))
  };

  // Deteksi bot (Google, Bing, Yandex)
  const agent = navigator.userAgent.toLowerCase();
  if (agent.includes('googlebot') || agent.includes('bingbot') || agent.includes('yandex')) {
    // Buat tampilan XML untuk bot
    const blob = new Blob([xml], { type: 'application/xml' });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'sitemap.xml';
    document.body.appendChild(link);
  } else {
    // Buat sitemap viewer kecil untuk manusia
    const box = document.createElement('div');
    box.style = "font-family:Arial;padding:15px;margin:15px;background:#fff;border-radius:10px;box-shadow:0 3px 8px rgba(0,0,0,0.2);";
    box.innerHTML = `
      <h3>🌍 Aleo's Tube Multilingual Sitemap</h3>
      <p>Diperbarui otomatis: ${new Date().toLocaleString()}</p>
      <details>
        <summary>Lihat Sitemap JSON</summary>
        <pre>${JSON.stringify(json, null, 2)}</pre>
      </details>
    `;
    document.body.appendChild(box);
  }
})();
</script>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='HTML3' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- Linktree Horizontal Oval dengan Avatar Efek Api + Spark -->
<div class="linktree-mini">
  <div class="avatar-fire">
    <img src="https://ugc.production.linktr.ee/6594230f-65d4-4070-9815-dba010c4b2b9_Aleos-Tube.png?io=true&size=avatar-v3_0" alt="Aleo's Tube" id="avatar" />
    <div class="flame flame1"></div>
    <div class="flame flame2"></div>
    <div class="flame flame3"></div>

   <!-- Sparks -->
    <span class="spark"></span>
    <span class="spark"></span>
    <span class="spark"></span>
    <span class="spark"></span>
    <span class="spark"></span>
  </div>

    <a 
href="https://youtube.com/UCTHGlP7T12oHv2QHDuw0C3g" class="btn btn-yt"><i class="fab fa-youtube"></i> YT</a>
    <a href="https://instagram.com/daily_vlog_anak_rantau" class="btn btn-ig"><i class="fab fa-instagram"></i> IG</a>
    <a 
href="https://facebook.com/aleostube" class="btn btn-fb"><i class="fab fa-facebook"></i> FB</a>
    <a 
href="https://pinterest.com/aleostube" class="btn btn-pin"><i class="fab fa-pinterest"></i> Pin</a>
    <a 
href="https://x.com/aleostube" class="btn btn-th"><i class="fab fa-x"></i> XT</a>
  <div class="btn-row">
</div></div>
  
<!-- CSS + Icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
  .linktree-mini {
    text-align: center;
    margin: 15px auto;
    background: transparent;
  }

  /* Avatar efek api */
  .avatar-fire {
    position: relative;
    display: inline-block;
    margin-bottom: 20px;
  }
  .avatar-fire img {
    width: 90px;
    height: 90px;
    border-radius: 50%;
    object-fit: cover;
    z-index: 3;
    position: relative;
  }

  /* Lidah api */
  .flame {
    position: absolute;
    top: -25px; left: -25px; right: -25px; bottom: -25px;
    border-radius: 50%;
    filter: blur(28px);
    z-index: 1;
    opacity: 0.7;
    animation: flicker 2s infinite ease-in-out alternate;
  }
  .flame1 {
    background: radial-gradient(circle, rgba(255,140,0,0.8) 20%, transparent 70%);
    animation-delay: 0s;
  }
  .flame2 {
    background: radial-gradient(circle, rgba(255,69,0,0.8) 15%, transparent 80%);
    animation-delay: 0.5s;
  }
  .flame3 {
    background: radial-gradient(circle, rgba(255,215,0,0.7) 10%, transparent 80%);
    animation-delay: 1s;
  }

  @keyframes flicker {
    0% { transform: scale(1) rotate(0deg); opacity: 0.7; }
    50% { transform: scale(1.2) rotate(12deg); opacity: 1; }
    100% { transform: scale(1) rotate(-12deg); opacity: 0.6; }
  }

  /* Sparks (percikan api) */
  .spark {
    position: absolute;
    width: 6px; height: 6px;
    background: radial-gradient(circle, #ffec85 20%, #ff9900 70%, transparent 100%);
    border-radius: 50%;
    animation: sparkFly 3s linear infinite;
    opacity: 0.8;
    z-index: 2;
  }
  .spark:nth-child(4) { top: 10%; left: 40%; animation-delay: 0s; }
  .spark:nth-child(5) { top: 30%; left: 70%; animation-delay: 0.5s; }
  .spark:nth-child(6) { top: 60%; left: 20%; animation-delay: 1s; }
  .spark:nth-child(7) { top: 80%; left: 50%; animation-delay: 1.5s; }
  .spark:nth-child(8) { top: 40%; left: 10%; animation-delay: 2s; }

  @keyframes sparkFly {
    0% { transform: translateY(0) scale(1); opacity: 0.9; }
    50% { transform: translateY(-40px) scale(0.8); opacity: 1; }
    100% { transform: translateY(-80px) scale(0.5); opacity: 0; }
  }

  /* Tombol oval horizontal */
  .btn-row {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 10px;
  }
  .btn {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 8px 14px;
    border-radius: 25px;
    font-size: 0.9rem;
    font-weight: 600;
    color: #fff;
    text-decoration: none;
    position: relative;
    overflow: hidden;
    z-index: 1;
  }
  .btn::before {
    content: "";
    position: absolute;
    inset: -2px;
    border-radius: 30px;
    background: linear-gradient(90deg, red, orange, yellow, green, blue, violet);
    background-size: 400% 100%;
    animation: rainbowMove 6s linear infinite;
    z-index: -2;
  }
  .btn::after {
    content: "";
    position: absolute;
    inset: 2px;
    border-radius: 25px;
    z-index: -1;
  }
  .btn-wa::after { background: #25D366; }
  .btn-yt::after { background: #FF0000; }
  .btn-ig::after { background: linear-gradient(45deg,#f58529,#dd2a7b,#8134af,#515bd4); }
  .btn-fb::after { background: #1877F2; }
  .btn-pin::after { background: #E60023; }
  .btn-th::after { background: #000; }

  .btn i { font-size: 1rem; }

  @keyframes rainbowMove {
    0% { background-position: 0% 50%; }
    100% { background-position: 100% 50%; }
  }
</style>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='ReportAbuse1' locked='false' title='' type='ReportAbuse'>
    <b:includable id='main'>
  <b:include name='reportAbuse'/>
</b:includable>
  </b:widget>
  <b:widget id='Label1' locked='false' title='Label' type='Label'>
    <b:widget-settings>
      <b:widget-setting name='sorting'>FREQUENCY</b:widget-setting>
      <b:widget-setting name='display'>CLOUD</b:widget-setting>
      <b:widget-setting name='selectedLabelsList'/>
      <b:widget-setting name='showType'>ALL</b:widget-setting>
      <b:widget-setting name='showFreqNumbers'>true</b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'>
    <h2><data:title/></h2>
  </b:if>
  <div expr:class='&quot;widget-content &quot; + data:display + &quot;-label-widget-content&quot;'>
    <b:if cond='data:display == &quot;list&quot;'>
      <ul>
        <b:loop values='data:labels' var='label'>
          <li>
            <b:if cond='data:blog.url == data:label.url'>
              <span expr:dir='data:blog.languageDirection'><data:label.name/></span>
            <b:else/>
              <a expr:dir='data:blog.languageDirection' expr:href='data:label.url'><data:label.name/></a>
            </b:if>
            <b:if cond='data:showFreqNumbers'>
              <span dir='ltr'>(<data:label.count/>)</span>
            </b:if>
          </li>
        </b:loop>
      </ul>
    <b:else/>
      <b:loop values='data:labels' var='label'>
        <span expr:class='&quot;label-size label-size-&quot; + data:label.cssSize'>
          <b:if cond='data:blog.url == data:label.url'>
            <span expr:dir='data:blog.languageDirection'><data:label.name/></span>
          <b:else/>
            <a expr:dir='data:blog.languageDirection' expr:href='data:label.url'><data:label.name/></a>
          </b:if>
          <b:if cond='data:showFreqNumbers'>
            <span class='label-count' dir='ltr'>(<data:label.count/>)</span>
          </b:if>
        </span>
      </b:loop>
    </b:if>
    <b:include name='quickedit'/>
  </div>
</b:includable>
  </b:widget>
  <b:widget id='BlogArchive1' locked='false' title='Arsip Blog' type='BlogArchive'>
    <b:widget-settings>
      <b:widget-setting name='showStyle'>MENU</b:widget-setting>
      <b:widget-setting name='yearPattern'>yyyy</b:widget-setting>
      <b:widget-setting name='showWeekEnd'>true</b:widget-setting>
      <b:widget-setting name='monthPattern'>MMMM yyyy</b:widget-setting>
      <b:widget-setting name='dayPattern'>MMM dd</b:widget-setting>
      <b:widget-setting name='weekPattern'>MM/dd</b:widget-setting>
      <b:widget-setting name='chronological'>true</b:widget-setting>
      <b:widget-setting name='showPosts'>false</b:widget-setting>
      <b:widget-setting name='frequency'>DAILY</b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'>
    <h2><data:title/></h2>
  </b:if>
  <div class='widget-content'>
  <div id='ArchiveList'>
  <div expr:id='data:widget.instanceId + &quot;_ArchiveList&quot;'>
    <b:include cond='data:style == &quot;HIERARCHY&quot;' data='data' name='interval'/>
    <b:include cond='data:style == &quot;FLAT&quot;' data='data' name='flat'/>
    <b:include cond='data:style == &quot;MENU&quot;' data='data' name='menu'/>
  </div>
  </div>
  <b:include name='quickedit'/>
  </div>
</b:includable>
    <b:includable id='flat' var='data'>
  <ul class='flat'>
    <b:loop values='data:data' var='i'>
      <li class='archivedate'>
        <a expr:href='data:i.url'><data:i.name/></a> (<data:i.post-count/>)
      </li>
    </b:loop>
  </ul>
</b:includable>
    <b:includable id='interval' var='intervalData'>
  <b:loop values='data:intervalData' var='interval'>
    <ul class='hierarchy'>
      <li expr:class='&quot;archivedate &quot; + data:interval.expclass'>
        <b:include cond='data:interval.toggleId' data='interval' name='toggle'/>
        <a class='post-count-link' expr:href='data:interval.url'>
          <data:interval.name/>
        </a>
        <span class='post-count' dir='ltr'>(<data:interval.post-count/>)</span>
        <b:include cond='data:interval.data' data='interval.data' name='interval'/>
        <b:include cond='data:interval.posts' data='interval.posts' name='posts'/>
      </li>
    </ul>
  </b:loop>
</b:includable>
    <b:includable id='menu' var='data'>
  <select expr:id='data:widget.instanceId + &quot;_ArchiveMenu&quot;'>
    <option value=''><data:title/></option>
    <b:loop values='data:data' var='i'>
      <option expr:value='data:i.url'><data:i.name/> (<data:i.post-count/>)</option>
    </b:loop>
  </select>
</b:includable>
    <b:includable id='posts' var='posts'>
  <ul class='posts'>
    <b:loop values='data:posts' var='post'>
      <li><a expr:href='data:post.url'><data:post.title/></a></li>
    </b:loop>
  </ul>
</b:includable>
    <b:includable id='toggle' var='interval'>
  <a class='toggle' href='javascript:void(0)'>
    <span expr:class='&quot;zippy&quot; + (data:interval.expclass == &quot;expanded&quot; ? &quot; toggle-open&quot; : &quot;&quot;)'>
      <b:if cond='data:interval.expclass == &quot;expanded&quot;'>
        &#9660;&#160;
      <b:elseif cond='data:blog.languageDirection == &quot;rtl&quot;'/>
        &#9668;&#160;
      <b:else/>
        &#9658;&#160;
      </b:if>
    </span>
  </a>
</b:includable>
  </b:widget>
  <b:widget id='Profile1' locked='false' title='Mengenai Saya' type='Profile'>
    <b:widget-settings>
      <b:widget-setting name='showaboutme'>true</b:widget-setting>
      <b:widget-setting name='showlocation'>true</b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
    <b:if cond='data:title != &quot;&quot;'>
      <h2><data:title/></h2>
    </b:if>
    <div class='widget-content'>
    <b:if cond='data:team'> <!-- team blog profile -->
      <ul>
        <b:loop values='data:authors' var='i'>
          <li><a class='profile-name-link g-profile' expr:href='data:i.userUrl' expr:style='&quot;background-image: url(&quot; + data:i.profileLogo + &quot;);&quot;'><data:i.display-name/></a></li>
        </b:loop>
      </ul>

    <b:else/> <!-- normal blog profile -->

      <b:if cond='data:photo.url != &quot;&quot;'>
        <a expr:href='data:userUrl'><img class='profile-img' expr:alt='data:messages.myPhoto' expr:height='data:photo.height' expr:src='data:photo.url' expr:width='data:photo.width'/></a>
      </b:if>

      <dl class='profile-datablock'>
        <dt class='profile-data'>
          <a class='profile-name-link g-profile' expr:href='data:userUrl' expr:style='&quot;background-image: url(&quot; + data:profileLogo + &quot;);&quot;' rel='author'>
            <data:displayname/>
          </a>
        </dt>

        <b:if cond='data:showlocation'>
          <dd class='profile-data'><data:location/></dd>
        </b:if>

        <b:if cond='data:aboutme != &quot;&quot;'><dd class='profile-textblock'><data:aboutme/></dd></b:if>
      </dl>
      <a class='profile-link' expr:href='data:userUrl' rel='author'><data:viewProfileMsg/></a>
    </b:if>

     <b:include name='quickedit'/>
    </div>
  </b:includable>
  </b:widget>
  <b:widget id='BlogSearch1' locked='false' title='Cari Blog Ini' type='BlogSearch'>
    <b:includable id='main'>
    <!-- only display title if it's non-empty -->
    <b:if cond='data:title != &quot;&quot;'>
      <h2 class='title'><data:title/></h2>
    </b:if>

    <div class='widget-content'>
      <div expr:id='data:widget.instanceId + &quot;_form&quot;'>
        <form class='gsc-search-box' expr:action='data:blog.searchUrl'>
          <b:attr cond='not data:view.isPreview' name='target' value='_top'/>
          <table cellpadding='0' cellspacing='0' class='gsc-search-box'>
            <tbody>
              <tr>
                <td class='gsc-input'>
                  <input autocomplete='off' class='gsc-input' expr:value='data:view.isSearch ? data:view.search.query.escaped : &quot;&quot;' name='q' size='10' title='search' type='text'/>
                </td>
                <td class='gsc-search-button'>
                  <input class='gsc-search-button' expr:value='data:messages.search' title='search' type='submit'/>
                </td>
              </tr>
            </tbody>
          </table>
        </form>
      </div>
    </div>

    <b:include name='quickedit'/>
  </b:includable>
  </b:widget>
  <b:widget id='PageList1' locked='false' title='' type='PageList'>
    <b:widget-settings>
      <b:widget-setting name='pageListJson'><![CDATA[{"link1":{"href":"https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/p/aleos-tube-daily-vlog-anak-rantau.html","position":1,"title":"Youtube | Aleo's Tube"},"link0":{"href":"http://aleos-tube-daily-vlog-anak-rantau.blogspot.com","position":0,"title":"Beranda"},"944015611032219360":{"href":"https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/p/fb-aleos-tube.html","position":2,"title":"Fb | Aleo's Tube"}}]]></b:widget-setting>
      <b:widget-setting name='homeTitle'>Beranda</b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'><h2><data:title/></h2></b:if>
  <div class='widget-content'>
    <b:if cond='data:mobile'>
      <select expr:id='data:widget.instanceId + &quot;_select&quot;'>
        <b:if cond='data:showPlaceholder'>
          <option disabled='disabled' hidden='hidden' value=''>
            <b:attr cond='!data:hasCurrentPage' name='selected' value='selected'/>
            <b:message name='messages.moveToPage'/>
          </option>
        </b:if>
        <b:loop values='data:links' var='link'>
          <option expr:value='data:link.href'>
            <b:attr cond='data:link.isCurrentPage' name='selected' value='selected'/>
            <data:link.title/>
          </option>
        </b:loop>
      </select>
      <span class='pagelist-arrow'>&amp;#9660;</span>
    <b:else/>
      <ul>
        <b:loop values='data:links' var='link'>
          <li>
            <b:class cond='data:link.isCurrentPage' name='selected'/>
            <a expr:href='data:link.href'><data:link.title/></a>
          </li>
        </b:loop>
      </ul>
    </b:if>
    <b:include name='quickedit'/>
  </div>
</b:includable>
  </b:widget>
  <b:widget id='HTML1' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- 🌍 Auto Translate + Widget Bahasa (Kiri Bawah) -->
<div id="google_translate_element" style="display:none;"></div>

<!-- Widget Bahasa Pengunjung -->
<div id="lang-widget"
     style="position:fixed;bottom:20px;left:20px;
            background:var(--widget-bg,#ffffffd9);
            color:var(--widget-text,#111);
            font-family:'Segoe UI',sans-serif;
            padding:10px 14px;
            border-radius:14px;
            box-shadow:0 4px 14px rgba(0,0,0,0.15);
            font-size:14px;
            z-index:9999;
            display:flex;
            align-items:center;
            gap:8px;
            backdrop-filter:blur(6px);
            opacity:0;
            transform:translateY(20px);
            transition:all 0.8s ease;">
  <span id="flag" style="font-size:18px;">🌍</span>
  Bahasa: <span id="detected-lang">Mendeteksi...</span>
</div>

<script type="text/javascript">
/* 🌐 Google Auto Translate */
function googleTranslateElementInit() {
  new google.translate.TranslateElement({
    pageLanguage: 'id', // Ganti sesuai bahasa utama blog
    layout: google.translate.TranslateElement.InlineLayout.SIMPLE,
    autoDisplay: true
  }, 'google_translate_element');
}

// Load Google Translate Script
(function() {
  const gt = document.createElement('script');
  gt.type = 'text/javascript';
  gt.async = true;
  gt.src = '//translate.google.com/translate_a/element.js?cb=googleTranslateElementInit';
  const s = document.getElementsByTagName('script')[0];
  s.parentNode.insertBefore(gt, s);
})();

/* 🌏 Deteksi bahasa + tampilkan nama & bendera */
const langToFlag = {
  id: "🇮🇩", en: "🇺🇸", tet: "🇹🇱", pt: "🇵🇹", es: "🇪🇸",
  fr: "🇫🇷", de: "🇩🇪", ja: "🇯🇵", zh: "🇨🇳", ko: "🇰🇷",
  ar: "🇸🇦", ru: "🇷🇺", it: "🇮🇹", tl: "🇵🇭", my: "🇲🇲",
  th: "🇹🇭", vi: "🇻🇳", ms: "🇲🇾", hi: "🇮🇳", nl: "🇳🇱",
  tr: "🇹🇷", fa: "🇮🇷", pl: "🇵🇱", uk: "🇺🇦", sv: "🇸🇪",
  fi: "🇫🇮", no: "🇳🇴", da: "🇩🇰", ro: "🇷🇴", el: "🇬🇷",
  he: "🇮🇱", hu: "🇭🇺", cs: "🇨🇿", sk: "🇸🇰", sr: "🇷🇸",
  bg: "🇧🇬", hr: "🇭🇷", sl: "🇸🇮", sq: "🇦🇱", lt: "🇱🇹",
  lv: "🇱🇻", et: "🇪🇪", sw: "🇰🇪", af: "🇿🇦", ne: "🇳🇵",
  ur: "🇵🇰", ps: "🇦🇫", km: "🇰🇭", lo: "🇱🇦", mn: "🇲🇳",
  ta: "🇮🇳", te: "🇮🇳", si: "🇱🇰", bn: "🇧🇩", kn: "🇮🇳"
  ang:"🇦🇴",
};

const userLang = (navigator.language || navigator.userLanguage).split('-')[0];
const langNames = new Intl.DisplayNames(['id'], { type: 'language' });
const detectedLangName = langNames.of(userLang) || userLang;

// Isi widget
document.getElementById('detected-lang').textContent = detectedLangName;
document.getElementById('flag').textContent = langToFlag[userLang] || "🌍";

// Animasi muncul lembut
const widget = document.getElementById('lang-widget');
setTimeout(() => {
  widget.style.opacity = '1';
  widget.style.transform = 'translateY(0)';
}, 800);

// 🌗 Tema Otomatis
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
if (prefersDark) {
  widget.style.setProperty('--widget-bg', '#1e1e1ed9');
  widget.style.setProperty('--widget-text', '#fff');
} else {
  widget.style.setProperty('--widget-bg', '#ffffffd9');
  widget.style.setProperty('--widget-text', '#111');
}

// Hover animasi lembut
widget.addEventListener('mouseenter', () => {
  widget.style.transform = 'scale(1.05)';
});
widget.addEventListener('mouseleave', () => {
  widget.style.transform = 'scale(1)';
});
</script>
<!-- 🌍 Auto Translate + Widget Bahasa (Kiri Bawah) -->
<div id="google_translate_element" style="display:none;"></div>

<!-- Widget Bahasa Pengunjung -->
<div id="lang-widget"
     style="position:fixed;bottom:20px;left:20px;
            background:var(--widget-bg,#ffffffd9);
            color:var(--widget-text,#111);
            font-family:'Segoe UI',sans-serif;
            padding:10px 14px;
            border-radius:14px;
            box-shadow:0 4px 14px rgba(0,0,0,0.15);
            font-size:14px;
            z-index:9999;
            display:flex;
            align-items:center;
            gap:8px;
            backdrop-filter:blur(6px);
            opacity:0;
            transform:translateY(20px);
            transition:all 0.8s ease;">
  <span id="flag" style="font-size:18px;">🌍</span>
  Bahasa: <span id="detected-lang">Mendeteksi...</span>
</div>

<script type="text/javascript">
/* 🌐 Google Auto Translate */
function googleTranslateElementInit() {
  new google.translate.TranslateElement({
    pageLanguage: 'id', // Ganti sesuai bahasa utama blog
    layout: google.translate.TranslateElement.InlineLayout.SIMPLE,
    autoDisplay: true
  }, 'google_translate_element');
}

// Load Google Translate Script
(function() {
  const gt = document.createElement('script');
  gt.type = 'text/javascript';
  gt.async = true;
  gt.src = '//translate.google.com/translate_a/element.js?cb=googleTranslateElementInit';
  const s = document.getElementsByTagName('script')[0];
  s.parentNode.insertBefore(gt, s);
})();

/* 🌏 Deteksi bahasa + tampilkan nama & bendera */
const langToFlag = {
  id: "🇮🇩", en: "🇺🇸", tet: "🇹🇱", pt: "🇵🇹", es: "🇪🇸",
  fr: "🇫🇷", de: "🇩🇪", ja: "🇯🇵", zh: "🇨🇳", ko: "🇰🇷",
  ar: "🇸🇦", ru: "🇷🇺", it: "🇮🇹", tl: "🇵🇭", my: "🇲🇲",
  th: "🇹🇭", vi: "🇻🇳", ms: "🇲🇾", hi: "🇮🇳", nl: "🇳🇱",
  tr: "🇹🇷", fa: "🇮🇷", pl: "🇵🇱", uk: "🇺🇦", sv: "🇸🇪",
  fi: "🇫🇮", no: "🇳🇴", da: "🇩🇰", ro: "🇷🇴", el: "🇬🇷",
  he: "🇮🇱", hu: "🇭🇺", cs: "🇨🇿", sk: "🇸🇰", sr: "🇷🇸",
  bg: "🇧🇬", hr: "🇭🇷", sl: "🇸🇮", sq: "🇦🇱", lt: "🇱🇹",
  lv: "🇱🇻", et: "🇪🇪", sw: "🇰🇪", af: "🇿🇦", ne: "🇳🇵",
  ur: "🇵🇰", ps: "🇦🇫", km: "🇰🇭", lo: "🇱🇦", mn: "🇲🇳",
  ta: "🇮🇳", te: "🇮🇳", si: "🇱🇰", bn: "🇧🇩", kn: "🇮🇳"
  ang:"🇦🇴",
};

const userLang = (navigator.language || navigator.userLanguage).split('-')[0];
const langNames = new Intl.DisplayNames(['id'], { type: 'language' });
const detectedLangName = langNames.of(userLang) || userLang;

// Isi widget
document.getElementById('detected-lang').textContent = detectedLangName;
document.getElementById('flag').textContent = langToFlag[userLang] || "🌍";

// Animasi muncul lembut
const widget = document.getElementById('lang-widget');
setTimeout(() => {
  widget.style.opacity = '1';
  widget.style.transform = 'translateY(0)';
}, 800);

// 🌗 Tema Otomatis
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
if (prefersDark) {
  widget.style.setProperty('--widget-bg', '#1e1e1ed9');
  widget.style.setProperty('--widget-text', '#fff');
} else {
  widget.style.setProperty('--widget-bg', '#ffffffd9');
  widget.style.setProperty('--widget-text', '#111');
}

// Hover animasi lembut
widget.addEventListener('mouseenter', () => {
  widget.style.transform = 'scale(1.05)';
});
widget.addEventListener('mouseleave', () => {
  widget.style.transform = 'scale(1)';
});
</script>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='HTML13' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[# Creating a single-file HTML for "Premium v8.3 Pulse Edition" (Page Mode) with:
# - Auto affiliate scanning (digistore24, shopee, tokopedia)
# - Sitemap HTML + XML generator
# - Auto-translate (Google Translate widget hidden + auto-select)
# - Subscribe popup linking to provided YouTube channel
# - Dark mode auto by visitor time + prefers-color-scheme fallback
html = """<!doctype html>

Pulse Edition v8.3 — Promo & Sitemap (Page Mode)
<style>
:root{
  --pulse-orange:#ff7a18; --pulse-blue:#1e90ff; --pulse-red:#ff3b30;
  --bg-dark:#0f1115; --bg-light:#f6f7fb; --card-radius:12px; --maxw:1100px;
  --shadow: 0 8px 30px rgba(2,6,23,0.18);
  --font: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
}
*{box-sizing:border-box}
html,body{height:100%;margin:0;font-family:var(--font);-webkit-font-smoothing:antialiased}
.container{max-width:var(--maxw);margin:18px auto;padding:18px}
.page-card{border-radius:var(--card-radius);padding:18px;box-shadow:var(--shadow);overflow:hidden;border:1px solid rgba(255,255,255,0.04)}
.header{display:flex;align-items:center;justify-content:space-between;gap:12px}
.title{font-size:20px;font-weight:800;margin:0;color:var(--pulse-orange)}
.subtitle{font-size:13px;color:rgba(255,255,255,0.75)}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px;margin-top:14px}
.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:12px;overflow:hidden;border:1px solid rgba(255,255,255,0.03)}
.thumb{width:100%;height:140px;object-fit:cover;background:#111;display:block}
.meta{padding:12px}
.title-sm{font-weight:700;font-size:14px;margin:0 0 6px}
.desc{font-size:13px;color:rgba(255,255,255,0.8);margin:0 0 10px}
.btn{display:inline-block;padding:8px 12px;border-radius:10px;background:linear-gradient(135deg,var(--pulse-orange),var(--pulse-blue));color:#fff;text-decoration:none;font-weight:700}
.small{font-size:12px;color:rgba(255,255,255,0.7)}
.controls{display:flex;gap:10px;align-items:center;margin-top:12px;flex-wrap:wrap}

/* sitemap */
.sitemap-list{list-style:none;padding:0;margin:0;display:flex;flex-direction:column;gap:6px}
.sitemap-list a{text-decoration:none;padding:8px;border-radius:8px;color:var(--pulse-blue);display:block}

/* modal subscribe */
.modal{position:fixed;inset:0;background:rgba(2,6,23,0.55);display:flex;align-items:center;justify-content:center;z-index:9999;padding:18px}
.modal-inner{width:100%;max-width:420px;background:linear-gradient(180deg,#0b0c0f,#0f1115);padding:18px;border-radius:12px;position:relative;box-shadow:0 12px 40px rgba(2,6,23,0.5);text-align:center}
.modal-close{position:absolute;right:10px;top:8px;border:0;background:transparent;color:#fff;font-size:18px;cursor:pointer}
.subscribe-btn{display:inline-block;padding:10px 14px;border-radius:10px;background:var(--pulse-red);color:#fff;text-decoration:none;font-weight:800;margin-top:12px}

/* hidden utility */
.hidden{display:none !important}

/* LIGHT THEME */
.light body, .light .page-card{background:var(--bg-light);color:#111}
.light .page-card{border:1px solid rgba(2,6,23,0.06)}
.light .subtitle{color:rgba(0,0,0,0.6)}
.light .desc{color:rgba(0,0,0,0.7)}

/* accessibility */
@media (prefers-reduced-motion: no-preference){
  .card{transition:transform .28s cubic-bezier(.22,.9,.24,1)}
  .card:hover{transform:translateY(-6px)}
}

/* hide Google Translate UI when it loads */
.goog-te-banner-frame.skiptranslate{display:none !important}
.goog-te-gadget-icon{display:none !important}
#google_translate_element{display:none}
</style>


<div class="container">
  <div id="pageCard" class="page-card">
    <div class="header">
      <div>
        <h1 class="title">Pulse Edition v8.3 — Promo & Sitemap</h1>
        <div class="subtitle small">Auto-affiliate, HTML & XML sitemap, auto-translate, dan ajakan subscribe Aleo's Tube.</div>
      </div>
      <div class="small">Warna: orange · biru · merah · transparan</div>
    </div>

    <div class="controls">
      <button id="openSitemapBtn" class="btn" style="background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--pulse-blue)">Tampilkan Sitemap</button>
      <button id="generateXmlBtn" class="btn">Generate XML Sitemap</button>
      <span class="small">Widget otomatis memindai posting untuk link afiliasi (digistore24, shopee, tokopedia).</span>
    </div>

    <div id="cardGrid" class="grid" aria-live="polite" style="margin-top:14px">
      <!-- cards injected -->
    </div>

    <div id="sitemapSection" class="hidden" style="margin-top:16px">
      <h3 style="margin:0 0 8px">HTML Sitemap</h3>
      <div id="sitemapOut" class="small sitemap-list">Sedang memuat…</div>
    </div>
  </div>
</div>

<!-- subscribe modal (hidden by default) -->
<div id="subscribeModal" class="modal hidden" role="dialog" aria-modal="true">
  <div class="modal-inner">
    <button class="modal-close" id="closeSubscribe">✕</button>
    <h3 style="margin-top:0">Dukung Aleo's Tube ❤️</h3>
    <p class="small">Jika suka konten, klik subscribe ke channel YouTube Aleo's Tube untuk update video terbaru.</p>
    <a id="subscribeLink" class="subscribe-btn" href="#" target="_blank" rel="noopener">Subscribe</a>
    <div style="margin-top:10px" class="small">Tidak ingin melihat lagi? Kamu bisa menutup popup ini.</div>
  </div>
</div>

<!-- hidden element for Google Translate initialisation -->
<div id="google_translate_element" class="hidden"></div>

<script>
(async function(){
  'use strict';
  // CONFIG
  const AFF_DOMAINS = ['digistore24.com','shopee.co.id','shopee.com','tokopedia.com'];
  const MAX_ITEMS = 9999;
  const BLOG_FEED = location.origin + '/feeds/posts/default?alt=json&max-results=9999';
  const YT_SUBSCRIBE = 'https://www.youtube.com/channel/UCTHGlP7T12oHv2QHDuw0C3g'; // from user
  const pageCard = document.getElementById('pageCard');
  const grid = document.getElementById('cardGrid');
  const sitemapOut = document.getElementById('sitemapOut');
  const sitemapSection = document.getElementById('sitemapSection');
  const openSitemapBtn = document.getElementById('openSitemapBtn');
  const generateXmlBtn = document.getElementById('generateXmlBtn');
  const subscribeModal = document.getElementById('subscribeModal');
  const subscribeLink = document.getElementById('subscribeLink');
  const closeSubscribe = document.getElementById('closeSubscribe');

  subscribeLink.href = YT_SUBSCRIBE;

  // small util
  const strip = s => (s||'').replace(/<[^>]+>/g,'').trim();

  // fetch blog feed
  async function fetchFeed(){
    try {
      const r = await fetch(BLOG_FEED, {cache: 'no-cache'});
      const j = await r.json();
      return j.feed && j.feed.entry ? j.feed.entry : [];
    } catch(e){
      console.warn('feed error', e);
      return [];
    }
  }

  function extractImage(entry){
    if(entry.media$thumbnail && entry.media$thumbnail.url) return entry.media$thumbnail.url.replace(/\\/s72-c\\//,'/s640/');
    const content = entry.content && entry.content.$t || entry.summary && entry.summary.$t || '';
    const m = content.match(/<img[^>]+src=['"]([^'"]+)['"]/i);
    if(m) return m[1];
    return null;
  }

  function findAffiliates(entry){
    const html = (entry.content && entry.content.$t) || (entry.summary && entry.summary.$t) || '';
    const urls = [];
    const re = /<a[^>]+href=['"]([^'"]+)['"]/g;
    let m;
    while((m=re.exec(html))!==null){
      try {
        const u = new URL(m[1], location.origin).href;
        for(const d of AFF_DOMAINS){
          if(u.includes(d)) urls.push(u);
        }
      } catch(e){}
    }
    return Array.from(new Set(urls));
  }

  function makeCard({title,desc,img,link}){
    const el = document.createElement('div');
    el.className = 'card';
    el.innerHTML = `<a href="${link}" target="_blank" rel="noopener" style="color:inherit;text-decoration:none;display:block">
      ${ img ? `<img class="thumb" loading="lazy" src="${img}" alt="${title}">` : `<div class="thumb" style="display:flex;align-items:center;justify-content:center;color:#fff"><svg width="120" height="80" xmlns="http://www.w3.org/2000/svg"><rect width="100%" height="100%" fill="#111"/></svg></div>` }
      <div class="meta">
        <div class="title-sm">${title}</div>
        <div class="desc">${desc}</div>
        <div style="margin-top:8px"><span class="btn">Beli / Lihat</span></div>
      </div>
    </a>`;
    return el;
  }

  // render UI
  async function render(){
    const entries = await fetchFeed();
    const found = [];
    const sitemapItems = [];
    for(const e of entries){
      const links = findAffiliates(e);
      const title = (e.title && e.title.$t) || 'Untitled';
      const url = (e.link || []).find(l=>l.rel==='alternate')?.href || '';
      sitemapItems.push({title,url,updated: e.published ? e.published.$t : (e.updated && e.updated.$t) || ''});
      if(links.length){
        found.push({
          title: strip(title),
          desc: strip((e.summary && e.summary.$t) || '').slice(0,140) || 'Produk afiliasi dari posting terkait',
          img: extractImage(e) || '',
          link: links[0]
        });
      }
    }

    // populate grid
    grid.innerHTML = '';
    if(found.length === 0){
      grid.innerHTML = '<div class="card" style="padding:18px">Belum ada link afiliasi terdeteksi pada posting. Pastikan link afiliasi muncul di konten posting dan domain terdaftar.</div>';
    } else {
      found.forEach(f=>grid.appendChild(makeCard(f)));
    }

    // sitemap out
    sitemapOut.innerHTML = sitemapItems.map(i=>`<a href="\${i.url}" target="_blank" rel="noopener">\${i.title||i.url}</a>`).join('');

    // attach to global for XML
    window.__pulse_sitemap = sitemapItems;
  }

  // generate XML
  function generateXML(items){
    const xmlItems = items.map(i=>{
      const loc = i.url;
      const lastmod = (i.updated) ? new Date(i.updated).toISOString() : new Date().toISOString();
      return `<url><loc>\${loc}</loc><lastmod>\${lastmod}</lastmod><changefreq>weekly</changefreq></url>`;
    }).join('');
    const xml = `<?xml version="1.0" encoding="UTF-8"?>\\n<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">\\n\${xmlItems}\\n</urlset>`;
    const w = window.open();
    w.document.open();
    w.document.write('<pre style="white-space:pre-wrap;word-break:break-all;background:#111;color:#fff;padding:16px;">' + escapeHtml(xml) + '</pre>');
    w.document.close();
  }

  function escapeHtml(s){ return (s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

  openSitemapBtn.addEventListener('click', ()=>{
    sitemapSection.classList.toggle('hidden');
    if(!sitemapSection.classList.contains('hidden')) window.scrollTo({top: sitemapSection.offsetTop - 20, behavior: 'smooth'});
  });

  generateXmlBtn.addEventListener('click', ()=> generateXML(window.__pulse_sitemap || []));

  // show subscribe modal after scroll 20% (once)
  let subShown = false;
  function onScrollShowSubscribe(){
    const scrolled = window.scrollY || window.pageYOffset;
    const docH = document.documentElement.scrollHeight - window.innerHeight;
    const ratio = docH>0 ? scrolled/docH : 0;
    if(ratio > 0.20 && !subShown){
      subShown = true;
      subscribeModal.classList.remove('hidden');
      window.removeEventListener('scroll', onScrollShowSubscribe);
    }
  }
  window.addEventListener('scroll', onScrollShowSubscribe);

  closeSubscribe.addEventListener('click', ()=> subscribeModal.classList.add('hidden'));

  // init
  await render();
  // refresh while open (low cost)
  setInterval(render, 1000 * 60 * 10);

  // =========================
  // Auto-translate (hidden UI)
  // We'll load Google Translate element and programmatically set target language to visitor language.
  // Note: this uses Google's public translate widget.
  window.googleTranslateElementInit = function() {
    try {
      new google.translate.TranslateElement({
        pageLanguage: 'id',
        layout: google.translate.TranslateElement.InlineLayout.SIMPLE,
        autoDisplay: false
      }, 'google_translate_element');

      // small delay then auto-select language matching navigator.language
      setTimeout(()=>{
        try {
          const lang = (navigator.language || navigator.userLanguage || 'en').split('-')[0];
          // find select within the iframe
          const frame = document.querySelector('.goog-te-menu-frame');
          if(frame){
            // attempt to change language via queryParameter trick (best-effort)
            // fallback: add url param to use translate.googleapis if possible
            // Many browsers block direct interaction; we attempt select change on the widget select if present
            const sel = document.querySelector('select.goog-te-combo');
            if(sel){
              sel.value = lang;
              sel.dispatchEvent(new Event('change'));
            }
          } else {
            const sel = document.querySelector('select.goog-te-combo');
            if(sel){
              sel.value = lang;
              sel.dispatchEvent(new Event('change'));
            }
          }
        } catch(e){/*fail silently*/ }
      }, 900);
    } catch(e){}
  };
  (function(){
    var gt = document.createElement('script'); gt.type='text/javascript'; gt.async=true;
    gt.src = 'https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit';
    document.head.appendChild(gt);
  })();

  // =========================
  // Auto dark/light mode based on visitor local time (00-06 light, 07-18 light, else dark) + prefers-color-scheme fallback
  function applyThemeByTime(){
    try {
      const now = new Date();
      const h = now.getHours();
      // default: day 7-18 => light, else dark
      const isLight = (h >= 7 && h <= 18);
      if(window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches){
        // respect user preference if system set to dark
        // but still apply time-based as primary (user preference will override if we invert)
      }
      if(isLight){
        document.documentElement.classList.add('light');
      } else {
        document.documentElement.classList.remove('light');
      }
    } catch(e){}
  }
  applyThemeByTime();

  // accessibility: respects prefers-reduced-motion already via CSS
})();
</script>


"""
# write file
path = "/mnt/data/v8p_pulse_v8_3_pagemode.html"
with open(path, "w", encoding="utf-8") as f:
    f.write(html)

print("File written to:", path)
print("Download link: sandbox:" + path)</!doctype>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='HTML4' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<style>
.mini-subscribe {
  position: fixed;
  right: 20px;
  bottom: 90px;
  background: red;
  color: #fff;
  font-size: 14px;
  padding: 6px 14px;
  border-radius: 30px;
  text-decoration: none;
  font-weight: bold;
  z-index: 9999;
  display: inline-block !important;
}
</style>

<a class="mini-subscribe" 
   href="https://www.youtube.com/UCTHGlP7T12oHv2QHDuw0C3g?sub_confirmation=1" 
   target="_blank">
   Subscribe
</a>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='HTML6' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- Ultra Mini Floating Playlist (Ukuran Tombol Subscribe) -->
<div class="yt-floating-mini">
  <iframe 
    src="https://www.youtube.com/embed/videoseries?list=PLSBXhP3KAxsDygkrgu8roI7YsOeUEUA4w&autoplay=1&mute=1&controls=0" 
    frameborder="0" 
    allow="autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
    allowfullscreen>
  </iframe>
</div>

<style>
.yt-floating-mini {
  position: fixed;
  bottom: 15px;      /* jarak dari bawah layar */
  right: 70px;       /* jarak dari kanan layar */
  width: 100px;      /* mirip tombol subscribe */
  border-radius: 30px;
  padding: 2px;
  background: linear-gradient(90deg, red, orange, yellow, green, cyan, blue, violet, red);
  background-size: 400% 400%;
  animation: rainbow 6s linear infinite, glow 2s ease-in-out infinite alternate;
  z-index: 9999;
  box-shadow: 0 2px 8px rgba(0,0,0,0.35);
}

.yt-floating-mini iframe {
  width: 100%;
  height: 40px;   /* tinggi kecil seperti tombol */
  border: none;
  border-radius: 30px;
  display: block;
}

@keyframes rainbow {
  0% {background-position: 0% 50%;}
  50% {background-position: 100% 50%;}
  100% {background-position: 0% 50%;}
}

@keyframes glow {
  0% {box-shadow: 0 0 4px rgba(255,255,255,0.3);}
  100% {box-shadow: 0 0 15px rgba(255,255,255,0.9);}
}
</style>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
</b:section-contents><b:section-contents id='footer-2-1'>
  <b:widget id='PageList2' locked='false' title='Halaman' type='PageList'>
    <b:widget-settings>
      <b:widget-setting name='pageListJson'><![CDATA[{"link1":{"href":"https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/p/aleos-tube-daily-vlog-anak-rantau.html","position":1,"title":"Youtube | Aleo's Tube"},"link0":{"href":"http://aleos-tube-daily-vlog-anak-rantau.blogspot.com/","position":0,"title":"Beranda"},"link3":{"href":"https://web.facebook.com/Aleostube/","position":3,"title":"Fb"},"link2":{"href":"https://id.pinterest.com/aleostube/","position":2,"title":"Pinterest"}}]]></b:widget-setting>
      <b:widget-setting name='homeTitle'>Beranda</b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'><h2><data:title/></h2></b:if>
  <div class='widget-content'>
    <b:if cond='data:mobile'>
      <select expr:id='data:widget.instanceId + &quot;_select&quot;'>
        <b:if cond='data:showPlaceholder'>
          <option disabled='disabled' hidden='hidden' value=''>
            <b:attr cond='!data:hasCurrentPage' name='selected' value='selected'/>
            <b:message name='messages.moveToPage'/>
          </option>
        </b:if>
        <b:loop values='data:links' var='link'>
          <option expr:value='data:link.href'>
            <b:attr cond='data:link.isCurrentPage' name='selected' value='selected'/>
            <data:link.title/>
          </option>
        </b:loop>
      </select>
      <span class='pagelist-arrow'>&amp;#9660;</span>
    <b:else/>
      <ul>
        <b:loop values='data:links' var='link'>
          <li>
            <b:class cond='data:link.isCurrentPage' name='selected'/>
            <a expr:href='data:link.href'><data:link.title/></a>
          </li>
        </b:loop>
      </ul>
    </b:if>
    <b:include name='quickedit'/>
  </div>
</b:includable>
  </b:widget>
  <b:widget id='Wikipedia1' locked='false' title='Wikipedia' type='Wikipedia'>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'><h2 class='title'><data:title/></h2></b:if>
  <div class='wikipedia-search-main-container'>
    <form class='wikipedia-search-form' expr:id='data:widget.instanceId + &quot;_wikipedia-search-form&quot;' name='wikipedia'>
      <div class='wikipedia-searchtable'>
        <span>
          <a class='wikipedia-search-wiki-link' href='https://wikipedia.org/wiki/' target='_blank'>
            <img align='top' class='wikipedia-icon' src='https://resources.blogblog.com/img/widgets/icon_wikipedia_w.png'/>
          </a>
        </span>
        <span class='wikipedia-search-bar'>
          <span class='wikipedia-input-box'>
            <input class='wikipedia-search-input' expr:id='data:widget.instanceId + &quot;_wikipedia-search-input&quot;' type='text'/>
          </span>
          <span>
            <input class='wikipedia-search-button' type='submit'/>
          </span>
        </span>
      </div>
    </form>
    <div class='wikipedia-search-results-header' expr:id='data:widget.instanceId + &quot;_wikipedia-search-results-header&quot;'><data:searchResultsMsg/></div>
    <div class='wikipedia-search-results' expr:id='data:widget.instanceId + &quot;_wikipedia-search-results&quot;'/>
    <nobr>
      <div expr:dir='data:blog.languageDirection' expr:id='data:widget.instanceId + &quot;_wikipedia-search-more&quot;'/>
    </nobr>
  </div><br/>
  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='ContactForm1' locked='false' title='Formulir Kontak' type='ContactForm'>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='contact-form-widget'>
    <div class='form'>
      <form name='contact-form'>
        <p/>
        <data:contactFormNameMsg/>
        <br/>
        <input class='contact-form-name' expr:id='data:widget.instanceId + &quot;_contact-form-name&quot;' name='name' size='30' type='text' value=''/>
        <p/>
        <data:contactFormEmailMsg/> <span style='font-weight: bolder;'>*</span>
        <br/>
        <input class='contact-form-email' expr:id='data:widget.instanceId + &quot;_contact-form-email&quot;' name='email' size='30' type='text' value=''/>
        <p/>
        <data:contactFormMessageMsg/> <span style='font-weight: bolder;'>*</span>
        <br/>
        <textarea class='contact-form-email-message' cols='25' expr:id='data:widget.instanceId + &quot;_contact-form-email-message&quot;' name='email-message' rows='5'/>
        <p/>
        <input class='contact-form-button contact-form-button-submit' expr:id='data:widget.instanceId + &quot;_contact-form-submit&quot;' expr:value='data:contactFormSendMsg' type='button'/>
        <p/>
        <div style='text-align: center; max-width: 222px; width: 100%'>
          <p class='contact-form-error-message' expr:id='data:widget.instanceId + &quot;_contact-form-error-message&quot;'/>
          <p class='contact-form-success-message' expr:id='data:widget.instanceId + &quot;_contact-form-success-message&quot;'/>
        </div>
      </form>
    </div>
  </div>
  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='PopularPosts2' locked='false' title='Postingan Populer' type='PopularPosts'>
    <b:widget-settings>
      <b:widget-setting name='numItemsToShow'>10</b:widget-setting>
      <b:widget-setting name='showThumbnails'>true</b:widget-setting>
      <b:widget-setting name='showSnippets'>true</b:widget-setting>
      <b:widget-setting name='timeRange'>ALL_TIME</b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'><h2><data:title/></h2></b:if>
  <div class='widget-content popular-posts'>
    <ul>
      <b:loop values='data:posts' var='post'>
      <li>
        <b:if cond='!data:showThumbnails'>
          <b:if cond='!data:showSnippets'>
            <!-- (1) No snippet/thumbnail -->
            <a expr:href='data:post.href'><data:post.title/></a>
          <b:else/>
            <!-- (2) Show only snippets -->
            <div class='item-title'><a expr:href='data:post.href'><data:post.title/></a></div>
            <div class='item-snippet'><data:post.snippet/></div>
          </b:if>
        <b:else/>
          <!-- (3) Show only thumbnails or (4) Snippets and thumbnails. -->
          <div expr:class='data:showSnippets ? &quot;item-content&quot; : &quot;item-thumbnail-only&quot;'>
            <b:if cond='data:post.featuredImage.isResizable or data:post.thumbnail'>
              <div class='item-thumbnail'>
                <a expr:href='data:post.href' target='_blank'>
                  <b:with value='data:post.featuredImage.isResizable                                  ? resizeImage(data:post.featuredImage, 72, &quot;1:1&quot;)                                  : data:post.thumbnail' var='image'>
                    <img alt='' border='0' expr:src='data:image'/>
                  </b:with>
                </a>
              </div>
            </b:if>
            <div class='item-title'><a expr:href='data:post.href'><data:post.title/></a></div>
            <b:if cond='data:showSnippets'>
              <div class='item-snippet'><data:post.snippet/></div>
            </b:if>
          </div>
          <div style='clear: both;'/>
        </b:if>
      </li>
      </b:loop>
    </ul>
    <b:include name='quickedit'/>
  </div>
</b:includable>
  </b:widget>
</b:section-contents><b:section-contents id='footer-2-2'>
  <b:widget id='HTML7' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- 💎 ALEO’S TUBE — YOUTUBE FEED WIDGET v3.0 -->
<script>
(async function(){
  const FEED_URL = 'https://www.youtube.com/feeds/videos.xml?channel_id=UCTHGlP7T12oHv2QHDuw0C3g';
  const PROXY = 'https://api.allorigins.win/get?url=' + encodeURIComponent(FEED_URL);
  const container = document.getElementById('ytVideos');

  try {
    const res = await fetch(PROXY);
    const data = await res.json();
    const parser = new DOMParser();
    const xml = parser.parseFromString(data.contents, 'application/xml');
    const entries = xml.getElementsByTagName('entry');

    const maxVideos = 6;
    container.innerHTML = '';

    for (let i = 0; i < Math.min(entries.length, maxVideos); i++) {
      const entry = entries[i];
      const title = entry.getElementsByTagName('title')[0].textContent;
      const link = entry.getElementsByTagName('link')[0].getAttribute('href');
      const idFull = entry.getElementsByTagName('id')[0].textContent;
      const videoId = idFull.split(':').pop();
      const thumb = `https://img.youtube.com/vi/${videoId}/hqdefault.jpg`;

      const videoCard = document.createElement('div');
      videoCard.style.cssText = `
        border-radius:12px;
        overflow:hidden;
        background:#f8f9fa;
        box-shadow:0 0 8px rgba(0,0,0,0.1);
        transition:transform 0.3s ease, box-shadow 0.3s ease;
      `;
      videoCard.innerHTML = `
        <a href="${link}" target="_blank" style="text-decoration:none;color:inherit;">
          <img src="${thumb}" alt="${title}" style="width:100%;display:block;">
          <div style="padding:10px;">
            <h4 style="font-size:15px;margin:0;">${title}</h4>
          </div>
        </a>
      `;
      videoCard.onmouseover = () => videoCard.style.transform = 'scale(1.03)';
      videoCard.onmouseout = () => videoCard.style.transform = 'scale(1)';
      container.appendChild(videoCard);
    }
  } catch (err) {
    container.innerHTML = '<p style="color:red;">Gagal memuat video 😢</p>';
    console.error('Feed error:', err);
  }
})();
</script>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='Subscribe1' locked='false' title='Langganan' type='Subscribe'>
    <b:includable id='main'>
  <div style='white-space:nowrap'>
    <b:if cond='data:title != &quot;&quot;'>
      <h2 class='title'><data:title/></h2>
    </b:if>
    <div class='widget-content'>
      <b:loop values='data:feeds' var='feed'>
        <div expr:class='&quot;subscribe-wrapper subscribe-type-&quot; + data:feed.type'>

          <div expr:class='&quot;subscribe expanded subscribe-type-&quot; + data:feed.type' expr:id='&quot;SW_READER_LIST_&quot; + data:widgetId + data:feed.type' style='display:none;'>
            <div class='top'>
              <span class='inner' expr:onclick='&quot;return(_SW_toggleReaderList(event, \&quot;&quot; + data:widgetId +data:feed.type + &quot;\&quot;));&quot;'>
                <img class='subscribe-dropdown-arrow' expr:src='data:arrowDropdownImg'/>
                <img align='absmiddle' alt='' border='0' class='feed-icon' expr:src='data:feedIconImg'/>
                <data:feed.title/>
              </span>

              <div class='feed-reader-links'>
                <a class='feed-reader-link' expr:href='&quot;https://www.netvibes.com/subscribe.php?url=&quot; + data:feed.encodedUrl' target='_blank'>
                  <img expr:src='data:imagePathBase + &quot;subscribe-netvibes.png&quot;'/>
                </a>
                <a class='feed-reader-link' expr:href='&quot;https://add.my.yahoo.com/content?url=&quot; + data:feed.encodedUrl' target='_blank'>
                  <img expr:src='data:imagePathBase + &quot;subscribe-yahoo.png&quot;'/>
                </a>
                <a class='feed-reader-link' expr:href='data:feed.url' target='_blank'>
                  <img align='absmiddle' class='feed-icon' expr:src='data:feedIconImg'/>
                  Atom
                </a>
              </div>

            </div>
            <div class='bottom'/>
          </div>

          <div class='subscribe' expr:id='&quot;SW_READER_LIST_CLOSED_&quot; + data:widgetId +data:feed.type' expr:onclick='&quot;return(_SW_toggleReaderList(event, \&quot;&quot; + data:widgetId +data:feed.type + &quot;\&quot;));&quot;'>
            <div class='top'>
               <span class='inner'>
                 <img class='subscribe-dropdown-arrow' expr:src='data:arrowDropdownImg'/>
                 <span expr:onclick='&quot;return(_SW_toggleReaderList(event, \&quot;&quot; + data:widgetId +data:feed.type + &quot;\&quot;));&quot;'>
                   <img align='absmiddle' alt='' border='0' class='feed-icon' expr:src='data:feedIconImg'/>
                   <data:feed.title/>
                 </span>
               </span>
             </div>
            <div class='bottom'/>
          </div>

        </div>
      </b:loop>

      <div style='clear:both'/>

    </div>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='Stats1' locked='false' title='Total Tayangan Halaman' type='Stats'>
    <b:widget-settings>
      <b:widget-setting name='showGraphicalCounter'>false</b:widget-setting>
      <b:widget-setting name='showAnimatedCounter'>false</b:widget-setting>
      <b:widget-setting name='showSparkline'>true</b:widget-setting>
      <b:widget-setting name='sparklineStyle'>BLACK_TRANSPARENT</b:widget-setting>
      <b:widget-setting name='timeRange'>ALL_TIME</b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <b:if cond='data:title != &quot;&quot;'><h2><data:title/></h2></b:if>
  <div class='widget-content'>
    <!-- Content is going to be visible when data will be fetched from server. -->
    <div expr:id='data:widget.instanceId + &quot;_content&quot;' style='display: none;'>
      <!-- Counter and image will be injected later via AJAX call. -->
      <b:if cond='data:showSparkline'>
        <script src='https://www.gstatic.com/charts/loader.js' type='text/javascript'/>
        <span expr:id='data:widget.instanceId + &quot;_sparklinespan&quot;' style='display:inline-block; width:75px; height:30px'/>
      </b:if>
      <span expr:class='&quot;counter-wrapper &quot; + (data:showGraphicalCounter ? &quot;graph-counter-wrapper&quot; : &quot;text-counter-wrapper&quot;)' expr:id='data:widget.instanceId + &quot;_totalCount&quot;'>
      </span>
      <b:include name='quickedit'/>
    </div>
  </div>
</b:includable>
  </b:widget>
</b:section-contents><b:section-contents id='footer-2-3'>
  <b:widget id='HTML8' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- Chat Bot Aleo's Tube v6 — Voice + Memory + Dark Mode + Auto-Translate + Subscribe -->
<div id="aleo-chat-root"></div>

<!-- Google Translate widget (will be loaded dynamically) -->
<div id="google_translate_element" style="display:none;"></div>

<style>
  :root {
    --primary-color: #2196f3;
    --bg: #ffffff;
    --text: #222;
    --muted: #666;
    --radius: 10px;
    --shadow: 0 6px 20px rgba(33,33,33,0.08);
    --max-width: 820px;
  }

  body.dark-mode {
    --bg: #121212;
    --text: #eee;
    --muted: #bbb;
    --shadow: 0 6px 18px rgba(0,0,0,0.4);
  }

  #aleo-chat-root {
    max-width: var(--max-width);
    margin: 28px auto;
    font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial;
    color: var(--text);
    position: relative;
  }

  .aleo-chat {
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    overflow: hidden;
    border: 1px solid rgba(0,0,0,0.05);
    display: flex;
    flex-direction: column;
    background: var(--bg);
  }

  .aleo-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 14px 16px;
    background: linear-gradient(90deg, var(--primary-color), rgba(0,0,0,0.1));
    color: white;
  }

  .aleo-header h3 { margin:0; font-size:16px; font-weight:600; }

  .aleo-header .btns { display:flex; gap:8px; align-items:center; }

  .aleo-header button, .aleo-header a.subscribe {
    background: rgba(255,255,255,0.15);
    color: white;
    border: none;
    border-radius: 6px;
    padding: 6px 10px;
    font-size: 12px;
    cursor: pointer;
    text-decoration: none;
    display:inline-flex;
    align-items:center;
    gap:8px;
  }
  .aleo-header button:hover, .aleo-header a.subscribe:hover { background: rgba(255,255,255,0.28); }

  .aleo-body {
    padding: 14px;
    min-height: 140px;
    max-height: 420px;
    overflow-y: auto;
    background: linear-gradient(180deg, rgba(0,0,0,0.02), transparent);
  }

  .msg { margin-bottom: 12px; display:flex; gap:8px; align-items:flex-end; }
  .msg.user { justify-content: flex-end; }

  .bubble {
    max-width: 84%;
    padding: 10px 12px;
    border-radius: 12px;
    line-height: 1.35;
    font-size: 14px;
    word-break: break-word;
  }

  .bubble.user {
    background: var(--primary-color);
    color: white;
    border-bottom-right-radius: 6px;
  }

  .bubble.bot {
    background: #f5f7fb;
    color: var(--text);
    border-bottom-left-radius: 6px;
  }

  body.dark-mode .bubble.bot {
    background: #1e1e1e;
    color: #eee;
  }

  .aleo-input {
    display:flex;
    gap:8px;
    padding: 12px;
    border-top: 1px solid rgba(0,0,0,0.04);
    background: var(--bg);
  }

  .aleo-input input[type="text"]{
    flex:1;
    padding:10px 12px;
    border-radius: 8px;
    border:1px solid #e6e9ef;
    font-size:14px;
    background: var(--bg);
    color: var(--text);
  }

  body.dark-mode .aleo-input input { border-color: #333; }

  .aleo-input button {
    background: var(--primary-color);
    color: white;
    border: none;
    padding: 9px 14px;
    border-radius: 8px;
    font-weight:600;
    cursor: pointer;
  }

  .aleo-footer {
    font-size:12px;
    color: var(--muted);
    padding: 8px 12px;
    text-align: right;
    background: var(--bg);
  }

  /* floating mini button (optional) */
  .aleo-floating {
    position: fixed;
    right: 18px;
    bottom: 18px;
    z-index: 9999;
  }
  .aleo-toggle {
    width:56px;height:56px;border-radius:50%;display:flex;align-items:center;justify-content:center;
    background:var(--primary-color);color:#fff;border:none;box-shadow:var(--shadow);cursor:pointer;font-weight:700;
    border: 3px solid rgba(255,255,255,0.06);
  }

  /* responsive */
  @media (max-width:600px) {
    #aleo-chat-root { margin: 16px; }
    .aleo-body { max-height: 320px; }
  }
</style>

<script>
(function(){
  // ---------- CONFIG ----------
  // Public channel link - converted from the studio link you gave
  const youtubeChannel = "https://www.youtube.com/channel/UCTHGlP7T12oHv2QHDuw0C3g";
  const youtubeSubscribeText = "Dukung Aleo's Tube di YouTube — klik Subscribe!";

  // Q&A
  const qaList = [
    { q: ["apa itu aleo's tube","tentang aleo","siapa aleo"], a: "Aleo's Tube adalah blog vlog anak rantau Indonesia — tempat berbagi kisah, pengalaman, dan keseharian hidup di luar negeri." },
    { q: ["siapa pemilik blog","siapa yang buat blog ini"], a: "Blog ini dikelola oleh Aleo, anak rantau Indonesia yang suka berbagi cerita dan tips hidup mandiri." },
    { q: ["tinggal di mana","lokasi aleo"], a: "Aleo sedang merantau di luar negeri dan sering berbagi pengalaman dari sana." },
    { q: ["jadwal upload","posting baru","kapan update"], a: "Biasanya ada posting atau vlog baru setiap minggu. Cek bagian 'Posting Terbaru' ya!" },
    { q: ["kategori konten","isi blog"], a: "Konten blog ini mencakup vlog harian, motivasi hidup, pengalaman merantau, dan tips menarik." },
    { q: ["komentar tidak muncul","cara komentar"], a: "Pastikan kamu login dengan akun Google sebelum berkomentar. Kadang butuh waktu untuk moderasi." },
    { q: ["follow","subscribe","langganan"], a: "Kamu bisa berlangganan lewat tombol 'Ikuti' di sidebar atau tombol Subscribe di channel YouTube kami." },
    { q: ["share","bagikan"], a: "Gunakan tombol share di bawah postingan untuk membagikan ke media sosial." },
    { q: ["lemot","error","tidak bisa dibuka"], a: "Coba muat ulang halaman atau hapus cache browser kamu. Jika masih error, laporkan di halaman Kontak." },
    { q: ["tema blog","desain"], a: "Tema blog ini berdesain minimalis, cepat, dan responsif di semua perangkat." },
    { q: ["kerjasama","endorse","afiliasi"], a: "Untuk kerja sama, silakan kirim email ke kontak@aleostube.com (ubah dengan email kamu sendiri)." },
    { q: ["kontak","hubungi","email"], a: "Hubungi Aleo lewat halaman Kontak di menu atas atau kirim email langsung ke kontak 👉aleostube@gmail.com." },
    { q: ["dukungan","donasi","support"], a: "Kamu bisa dukung blog ini dengan share, komentar positif, atau subscribe ke channel YouTube kami 🙏" },
    { q: ["motivasi hidup","semangat"], a: "Terus semangat! Hidup adalah perjalanan yang berharga, nikmati setiap langkahnya ✨" },
    { q: ["terima kasih","makasih"], a: "Sama-sama! Terima kasih sudah mampir ke Aleo’s Tube 💙" },
    { q: ["bye","dadah","sampai jumpa"], a: "Sampai jumpa lagi! Jangan lupa mampir ke postingan terbaru ya 👋" }
  ];

  const defaultAnswer = "Maaf, saya belum punya jawaban untuk itu 😅. Coba tanyakan hal lain atau kunjungi halaman Kontak untuk info lebih lengkap.";
  const storageKey = "aleo_chat_history_v6";

  // ---------- UTIL ----------
  function normalize(text){ return (text||"").toLowerCase().replace(/[^\w\s']/g, "").trim(); }
  function findAnswer(text){
    const t = normalize(text);
    for (const item of qaList){
      for (const kw of item.q){
        if (t.includes(normalize(kw))) return item.a;
      }
    }
    return null;
  }

  // ---------- TTS (santai & natural) ----------
  function speak(text){
    if (!window.speechSynthesis) return;
    const u = new SpeechSynthesisUtterance(text);
    u.lang = "id-ID";
    u.pitch = 1.05;
    u.rate = 1.0;
    u.volume = 1;
    const voices = speechSynthesis.getVoices();
    const voice = voices.find(v => v.lang && v.lang.startsWith("id")) || voices[0];
    if (voice) u.voice = voice;
    window.speechSynthesis.cancel();
    window.speechSynthesis.speak(u);
  }

  // ---------- BUILD UI ----------
  const root = document.getElementById('aleo-chat-root');
  root.innerHTML = `
    <div class="aleo-chat" id="aleo-chat">
      <div class="aleo-header">
        <h3>Aleo’s Tube — Chat Bantuan</h3>
        <div class="btns">
          <a class="subscribe" id="subscribeBtn" href="${youtubeChannel}" target="_blank" rel="noopener">
            📺 Subscribe
          </a>
          <button id="clearChat">Hapus Chat</button>
        </div>
      </div>
      <div class="aleo-body" id="chat-body" aria-live="polite"></div>
      <div class="aleo-input">
        <input id="chat-input" type="text" placeholder="Tanyakan sesuatu... (misal: 'Apa itu Aleo's Tube?')">
        <button id="chat-send">Kirim</button>
      </div>
      <div class="aleo-footer">🎙️ Chat otomatis — suara santai • Auto-translate tersedia</div>
    </div>

    <!-- floating toggle -->
    <div class="aleo-floating" id="aleo-floating" style="display:none;">
      <button class="aleo-toggle" id="aleo-open">AT</button>
    </div>
  `;

  const chatWrap = root.querySelector('#aleo-chat');
  const bodyEl = root.querySelector('#chat-body');
  const inputEl = root.querySelector('#chat-input');
  const sendBtn = root.querySelector('#chat-send');
  const clearBtn = root.querySelector('#clearChat');
  const subscribeBtn = root.querySelector('#subscribeBtn');
  const floating = root.querySelector('#aleo-floating');
  const openBtn = root.querySelector('#aleo-open');

  // ---------- HISTORY ----------
  function saveHistory(){ localStorage.setItem(storageKey, bodyEl.innerHTML); }
  function loadHistory(){
    const saved = localStorage.getItem(storageKey);
    if (saved) bodyEl.innerHTML = saved;
    else {
      appendBot("Halo 👋! Saya asisten santai Aleo’s Tube. Mau tanya apa hari ini?", true, false);
      // after greeting, show subscribe nudge
      setTimeout(()=> appendBot(youtubeSubscribeText + " — klik tombol Subscribe di atas.", true), 900);
    }
  }
  function clearHistory(){
    localStorage.removeItem(storageKey);
    bodyEl.innerHTML = "";
    appendBot("Percakapan dihapus. Yuk mulai chat baru 👋", true, false);
  }

  // ---------- MESSAGES ----------
  function appendUser(text){
    const wrap = document.createElement('div'); wrap.className = 'msg user';
    const b = document.createElement('div'); b.className='bubble user'; b.innerText = text;
    wrap.appendChild(b); bodyEl.appendChild(wrap); bodyEl.scrollTop = bodyEl.scrollHeight; saveHistory();
  }
  function appendBot(text, speakNow=true, save=true){
    const wrap = document.createElement('div'); wrap.className = 'msg bot';
    const b = document.createElement('div'); b.className='bubble bot'; b.innerText = text;
    wrap.appendChild(b); bodyEl.appendChild(wrap); bodyEl.scrollTop = bodyEl.scrollHeight;
    if (speakNow) speak(text);
    if (save) saveHistory();
  }

  // ---------- SEND HANDLER ----------
  function handleSend(){
    const t = inputEl.value.trim();
    if (!t) return;
    appendUser(t);
    inputEl.value = '';
    // special: if user asks to subscribe or mentions youtube
    const tl = normalize(t);
    if (tl.includes("subscribe") || tl.includes("youtube") || tl.includes("channel")) {
      setTimeout(()=> appendBot("Makasih! Kamu bisa subscribe di sini: " + youtubeChannel + " — terima kasih atas dukungannya! 🙏"), 400);
      return;
    }
    const ans = findAnswer(t) || defaultAnswer;
    setTimeout(()=> appendBot(ans, true), 450 + Math.random()*300);
  }

  sendBtn.addEventListener('click', handleSend);
  inputEl.addEventListener('keydown', e => { if (e.key === 'Enter') handleSend(); });
  clearBtn.addEventListener('click', clearHistory);

  // Floating open/close
  let collapsed = false;
  function showFloating(){ floating.style.display = ''; chatWrap.style.display = 'none'; collapsed = true; }
  function showChat(){ chatWrap.style.display = ''; floating.style.display = 'none'; collapsed = false; }
  openBtn && openBtn.addEventListener('click', ()=> showChat());

  // If screen small, show floating toggle
  if (window.innerWidth <= 900) {
    showFloating();
  }

  // ---------- AUTO TRANSLATE (Google Translate widget) ----------
  // We'll load the Google Translate element and try to auto-select visitor language.
  // NOTE: This uses the official Google Translate website widget (client-side).
  function loadGoogleTranslate(){
    if (window.google && window.google.translate) return initGTranslate();
    const s = document.createElement('script');
    s.src = "//translate.google.com/translate_a/element.js?cb=googleTranslateElementInit";
    s.async = true;
    document.head.appendChild(s);
    window.googleTranslateElementInit = initGTranslate;
  }

  function initGTranslate(){
    try {
      new google.translate.TranslateElement({
        pageLanguage: 'id',
        autoDisplay: false
      }, 'google_translate_element');

      // attempt to auto-select the user's language from navigator.language
      const userLang = (navigator.language || navigator.userLanguage || 'en').split('-')[0];
      setTimeout(()=> trySetTranslateLang(userLang), 600);
    } catch(e){
      // ignore if widget blocked
      console.warn("Google Translate init failed", e);
    }
  }

  function trySetTranslateLang(lang){
    // widget creates a select in iframe; try to find and set it
    const interval = setInterval(()=>{
      const gtFrame = document.querySelector('iframe.goog-te-menu-frame');
      if (!gtFrame) return;
      try {
        const doc = gtFrame.contentDocument || gtFrame.contentWindow.document;
        const select = doc.querySelector('select.goog-te-combo');
        if (select) {
          // map two-letter to available select options
          const opt = Array.from(select.options).find(o => o.value.indexOf(lang) === 0 || o.value.indexOf(lang+'-') === 0);
          if (opt) {
            select.value = opt.value;
            // dispatch change
            const ev = doc.createEvent('HTMLEvents'); ev.initEvent('change', true, true); select.dispatchEvent(ev);
            clearInterval(interval);
          }
        }
      } catch(err){
        // cross-origin may block until iframe loaded; ignore
      }
    }, 500);

    // Stop trying after 8s
    setTimeout(()=> clearInterval(interval), 8000);
  }

  // Expose a small API to toggle translate manually
  window.AleoChatTranslate = {
    enable: loadGoogleTranslate
  };

  // ---------- INIT ----------
  loadHistory();

  // Auto-load Google Translate (try)
  loadGoogleTranslate();

  // small helpful nudge: when chat loads, show subscribe call-to-action in UI (accessible)
  subscribeBtn.addEventListener('click', function(){
    // Also announce in chat
    appendBot("Makasih! Kalau sudah subscribe, tulis 'sudah subscribe' ya — biar saya tahu kamu dukung channel ini 🙂", false);
  });

  // ensure voices list loaded for some browsers
  if (typeof speechSynthesis !== "undefined" && speechSynthesis.onvoiceschanged !== undefined) {
    speechSynthesis.onvoiceschanged = () => {};
  }

  // accessibility: focus input on ctrl/cmd+k
  document.addEventListener('keydown', function(e){
    if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === 'k') {
      e.preventDefault();
      inputEl.focus();
    }
  });

})();
</script>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='HTML9' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<!-- Aleo's Tube - Widget Follow Blog Responsif + Auto Translate -->
<style type='text/css'><![CDATA[
.aleo-follow-widget {
    position: fixed;
    top: 50%;
    right: -300px;
    transform: translateY(-50%);
    width: 280px;
    max-width: 90vw;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: 15px 0 0 15px;
    box-shadow: -5px 0 25px rgba(0,0,0,0.4);
    z-index: 99999;
    transition: right 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    font-family: Arial, sans-serif;
    color: #fff;
    overflow: hidden;
}
.aleo-follow-widget.show { right: 0; }
.follow-header {
    background: rgba(255,255,255,0.1);
    padding: 15px;
    text-align: center;
    border-bottom: 1px solid rgba(255,255,255,0.15);
    position: relative;
}
.follow-header h3 {
    margin: 0;
    font-size: 16px;
    font-weight: bold;
}
.follow-content {
    padding: 20px 15px;
    text-align: center;
}
.blog-avatar {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    margin: 0 auto 12px;
    border: 3px solid rgba(255,255,255,0.3);
    background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 28px;
}
.blog-title {
    font-size: 14px;
    font-weight: bold;
    margin-bottom: 8px;
}
.blog-desc {
    font-size: 11px;
    opacity: 0.95;
    margin-bottom: 15px;
    line-height: 1.4;
}
.follow-btn {
    background: #ff6b6b;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 25px;
    font-size: 12px;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s;
    text-decoration: none;
    display: inline-block;
    margin-bottom: 10px;
}
.follow-btn:hover {
    background: #ff5252;
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(255,107,107,0.4);
}
.social-links {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 10px;
}
.social-links a {
    color: white;
    font-size: 16px;
    opacity: 0.8;
    transition: opacity 0.3s;
    text-decoration: none;
    padding: 8px;
    border-radius: 50%;
    background: rgba(255,255,255,0.1);
    width: 36px; height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
}
.social-links a:hover { opacity: 1; background: rgba(255,255,255,0.2);}
.close-widget {
    position: absolute;
    top: 8px;
    right: 12px;
    background: rgba(255,255,255,0.2);
    border: none;
    color: white;
    font-size: 16px;
    cursor: pointer;
    opacity: 0.8;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s;
}
.close-widget:hover { opacity: 1; }
.toggle-btn {
    position: fixed;
    top: 50%;
    right: 10px;
    transform: translateY(-50%);
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    border: none;
    width: 48px; height: 48px;
    border-radius: 50%;
    cursor: pointer;
    z-index: 100000;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    font-size: 22px;
    display: flex;
    align-items: center;
    justify-content: center;
}
@media (max-width: 600px) {
    .aleo-follow-widget { width: 95vw; right: -100vw; }
}
]]]]><![CDATA[></style>

<!-- HTML Widget -->
<div class="aleo-follow-widget" id="followWidget">
    <button class="close-widget" onclick="closeFollowWidget()">&#215;</button>
    <div class="follow-header">
        <h3 id="widget-title">🌟 Ikuti Blog Ini!</h3>
    </div>
    <div class="follow-content">
        <div class="blog-avatar">🌴</div>
        <div class="blog-title" id="widget-blog-title">Aleo’s Tube Daily Vlog</div>
        <div class="blog-desc" id="widget-desc">
            Cerita keseharian anak rantau di perkebunan sawit &amp; petualangan seru di desa!
        </div>
        <a href="https://aleos-tube-daily-vlog-anak-rantau.blogspot.com/feeds/posts/default" 
           class="follow-btn" target="_blank" id="widget-follow-btn">
           📧Ikuti Blog
        </a>
        <div class="social-links">
            <a href="https://www.youtube.com/UCTHGlP7T12oHv2QHDuw0C3g" target="_blank" title="YouTube">&#128250;</a>
            <a href="#" target="_blank" title="Instagram">&#128247;</a>
            <a href="#" target="_blank" title="Facebook">&#128100;</a>
        </div>
 <script>   
<button class="toggle-btn" id="toggleBtn" onclick="toggleFollowWidget()">👋</button>
</script></div></div>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
  <b:widget id='HTML10' locked='false' title='' type='HTML'>
    <b:widget-settings>
      <b:widget-setting name='content'><![CDATA[<style>
  /* Wrapper grid */
  .aleo-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }

  /* Card styling */
  .aleo-card {
    background: #11152a;
    border-radius: 12px;
    padding: 16px;
    color: #f1f1f1;
    box-shadow: 0 4px 15px rgba(0,180,255,0.2);
    transition: transform 0.3s, box-shadow 0.3s;
  }

  .aleo-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0,180,255,0.4);
  }

  .aleo-card img {
    max-width: 100%;
    border-radius: 8px;
    display: block;
    margin-bottom: 12px;
  }

  .aleo-card h2 {
    font-size: 1.2rem;
    margin: 0 0 10px 0;
    color: #00b0ff;
  }

  .aleo-card p {
    font-size: 0.95rem;
    line-height: 1.4;
  }
</style>

<div class="aleo-grid" id="aleoGrid">
  <!-- Konten akan otomatis ditempatkan di sini -->
</div>

<script>
  // Ambil semua post-body dan title dari blog
  const posts = document.querySelectorAll('.post');
  const grid = document.getElementById('aleoGrid');

  posts.forEach(post => {
    const card = document.createElement('div');
    card.className = 'aleo-card';

    // Ambil judul
    const title = post.querySelector('.post-title')?.innerHTML || '';
    const body = post.querySelector('.post-body')?.innerHTML || '';

    card.innerHTML = `
      <h2>${title}</h2>
      <div>${body}</div>
    `;

    grid.appendChild(card);
  });

  // Sembunyikan postingan asli supaya tidak dobel
  posts.forEach(p => p.style.display = 'none');
</script>]]></b:widget-setting>
    </b:widget-settings>
    <b:includable id='main'>
  <!-- only display title if it's non-empty -->
  <b:if cond='data:title != &quot;&quot;'>
    <h2 class='title'><data:title/></h2>
  </b:if>
  <div class='widget-content'>
    <data:content/>
  </div>

  <b:include name='quickedit'/>
</b:includable>
  </b:widget>
</b:section-contents></html>
