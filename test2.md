---
title: test 2
---
# some test document, test 2

without using the ace editor

<div id="doc1">

```latex
\documentclass{article}

\begin{document}

\section{The start}
aaa

\section{The End}
zzz
\end{document}
```

</div>


  <div id="buttons">
    <button id="overleaf" onclick="document.location='https://www.overleaf.com/read/kstsvwcdpqqm'" >Open in OverLeaf</button>
    <button id="compile" autofocus>Use texlive.js</button>
    <button id="open_pdf_btn" style="display: none">Open PDF</button>
  </div>





With a LaTeX example 3

and links to texlive.js and OverLeaf

  <footer>
    <div class="both">
      <h3>Console Output</h3>
      <pre id="output" style="overflow: scroll; font-size:12px; max-height: 7em">Click "Use texlive.js" and watch the console output here.</pre>
      <a name="running" id="running" style="display: none">Compiling...<img src="loading.gif" /></a>
    </div>


<script src="pdftex.js"></script>
<script>

  var appendOutput = function(msg) {
    var content = document.getElementById("output").textContent;

    var output = document.getElementById("output");
    output.textContent = content + "\r\n" + msg;

    output.scrollTop = 999999;
    console.log(msg);
  }

  var pdf_dataurl = undefined;
  var compile = function(source_code) {
    document.getElementById("output").textContent = "";


    var texlive = new TeXLive();
    var pdftex = texlive.pdftex;
    pdftex.on_stdout = appendOutput;
    pdftex.on_stderr = appendOutput;

    var start_time = new Date().getTime();

    pdftex.compile(source_code).then(function(pdf_dataurl) {
      var end_time = new Date().getTime();
      console.info("Execution time: " + (end_time - start_time) / 1000 + ' sec');


      if (pdf_dataurl === false)
        return;
      document.getElementById("open_pdf_btn").focus();
      texlive.terminate();
    });
  }

  document.getElementById("compile").addEventListener("click", function(e) {
    var source_code = doc1.textContent;
    compile(source_code);
  });

  document.getElementById("open_pdf_btn").addEventListener("click", function(e) {
    window.open(pdf_dataurl);
    e.preventDefault();
  });

  //var pdftex_preload = new PDFTeX("pdftex-worker.js");
  pdftex_preload = undefined;
</script>
