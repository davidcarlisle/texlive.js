---
title: test 1
---
<style>
    #editor {
      position: absolute;
      top: 7em;
      bottom: 21em;
      left: 0;
      right: 0;
    }

#embedded_ace_code {
    height: 300px;
    border: 1px solid #DDD;
    border-radius: 4px;
    border-bottom-right-radius: 0px;
    margin-top: 5px;
}

#ace_editor_demo {
    margin-bottom: 0px;
}


</style>

# test 1

Example document supposed to be here (not top of page)

<pre id="embedded_ace_code">
\documentclass{article}

\begin{document}

\section{The start}
aaa

\section{The End}
zzz
\end{document}
</pre>



  <div id="buttons">
    <button id="overleaf" onclick="document.location='https://www.overleaf.com/read/kstsvwcdpqqm'" >Open in OverLeaf</button>
    <button id="compile" autofocus>Use texlive.js</button>
    <button id="open_pdf_btn" style="display: none">Open PDF</button>
  </div>



# some test document

With a LaTeX example

and links to texlive.js and OverLeaf

  <footer>
    <div class="both">
      <h3>Console Output</h3>
      <pre id="output" style="overflow: scroll; font-size:12px; max-height: 7em">Click "Use texlive.js" and watch the console output here.</pre>
      <a name="running" id="running" style="display: none">Compiling...<img src="loading.gif" /></a>
    </div>

<script src="https://cdn.jsdelivr.net/g/ace@1.2.1(min/ace.js+min/ext-language_tools.js+min/mode-latex.js+min/snippets/latex.js)"></script>

<script src="complete/AutoComplete.js"></script>
<script src="promisejs/promise.js"></script>
<script src="pdftex.js"></script>
<script>
  var editor = ace.edit("embedded_ace_code");

  editor.setOptions({
    mode: "ace/mode/latex",
    fontSize: 14,
    hScrollBarAlwaysVisible: false,
    vScrollBarAlwaysVisible: true,
    indentedSoftWrap: true,
    printMargin: false,
    printMarginColumn: false,
    tabSize: 4,
    useSoftTabs: true,
  });

  var langTools = ace.require("ace/ext/language_tools")
  var AM = ace.require("complete/AutoCompleteManager");
  var AutoCompleteManager = new AM.AutoCompleteManager(editor);
  AutoCompleteManager.enable();

  editor.setOptions({
    enableBasicAutocompletion: true,
    enableLiveAutocompletion: true,
    enableSnippets: true,
  });

  var visibilityChanger = function(element_id) {
    return function(visible) {
      document.getElementById(element_id).style.display = visible ? 'inline' : 'none';
    }
  }

  var showLoadingIndicator = visibilityChanger("running")
  var showOpenButton = visibilityChanger("open_pdf_btn")

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
    showLoadingIndicator(true);

    var texlive = new TeXLive();
    var pdftex = texlive.pdftex;
    pdftex.on_stdout = appendOutput;
    pdftex.on_stderr = appendOutput;

    var start_time = new Date().getTime();

    pdftex.compile(source_code).then(function(pdf_dataurl) {
      var end_time = new Date().getTime();
      console.info("Execution time: " + (end_time - start_time) / 1000 + ' sec');

      showLoadingIndicator(false);

      if (pdf_dataurl === false)
        return;
      showOpenButton(true);
      document.getElementById("open_pdf_btn").focus();
      texlive.terminate();
    });
  }

  document.getElementById("compile").addEventListener("click", function(e) {
    var source_code = editor.getValue();
    compile(source_code);
  });

  document.getElementById("open_pdf_btn").addEventListener("click", function(e) {
    window.open(pdf_dataurl);
    e.preventDefault();
  });

  //var pdftex_preload = new PDFTeX("pdftex-worker.js");
  pdftex_preload = undefined;
</script>
