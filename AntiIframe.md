Afegeix aquestes línies dins de l'etiqueta <head> de cada nou fitxer HTML:

1) Capçalera CSP (recomanada al servidor però útil també com a meta):

<meta http-equiv="Content-Security-Policy" content="frame-ancestors 'self' https://sites.google.com/inslasagrera.cat/matessite2425/inici-mates;" />

2) Script anti-iframe i bloqueig de Ctrl+S/contextmenu (posa-ho també dins de <head>):

<script>
  (function(){
    const ALLOWED = 'https://sites.google.com/inslasagrera.cat/matessite2425/inici-mates';
    if (window.top !== window.self) {
      try {
        if (!window.top.location.href.includes(ALLOWED)) window.top.location = ALLOWED;
      } catch(e) { try { window.top.location = ALLOWED; } catch(_){} }
    }
    document.addEventListener('keydown', e => {
      const k = (e.key || '').toLowerCase();
      if ((e.ctrlKey || e.metaKey) && (k === 's' || k === 'u')) e.preventDefault();
    }, { passive: false });
    document.addEventListener('contextmenu', e => e.preventDefault(), { passive: false });
    document.addEventListener('selectstart', e => e.preventDefault(), { passive: false });
  })();
</script>

Nota: la mesura més fiable és configurar la capçalera HTTP `Content-Security-Policy: frame-ancestors ...` al teu servidor (GitHub Pages/NGINX/Apache). Les proteccions client-side dificulten però no impedeixen completament la descàrrega o captura per un usuari determinat.
