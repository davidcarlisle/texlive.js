---
title: test 3
---
# some test document, test 3

A description of  **LaTeX** in [Markdown (GFM)](https://github.github.com/gfm/)

With a code block using LaTeX syntax highlighting

* The <button style="padding:0;font-size:75%">Open in OverLeaf</button> button opens the example in Overleaf (currently a read-only instance under my account)
 * The <button style="padding:0;font-size:75%">Edit for texlive.js</button> button makes the code block editable and provides a button to compile it to PDF using texlive.js (running in your browser). A text box showing the console output will be displayed, and also a button to display the generated PDF.


```latex
\documentclass{article}

\begin{document}

\section{The start}
aaa

\section{The End}
zzz
\end{document}
```

<div id="texliverun" style="display: none" class="both">
<b>Console Output</b>
 <pre id="output" style="overflow: scroll; font-size:12px; max-height: 7em">Click "Use texlive.js" and watch the console output here.</pre>
   <a name="running" id="running" style="display: none">Compiling...<img src="loading.gif" /></a>
</div>



  <div id="buttons">
    <button id="overleaf" onclick="document.location='https://www.overleaf.com/read/kstsvwcdpqqm'" >Open in OverLeaf</button>
    <button id="edit" autofocus>Edit for texlive.js</button>
    <button id="compile"  style="display: none">Use texlive.js</button>
    <button id="open_pdf_btn" style="display: none">Open PDF</button>
  </div>





## More details

There is more markdown text here.




<script src="promisejs/promise.js"></script>
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
    var source_code = buttons.parentNode.getElementsByTagName("div")[0].textContent;
    compile(source_code);
  });

  document.getElementById("open_pdf_btn").addEventListener("click", function(e) {
//1    window.open(pdf_dataurl);
//2 var iframe = "<iframe width='100%' height='100%' src='"+ encodeURIComponent(pdf_dataurl) +  "'></iframe>"
//2 var x = window.open();
//2 x.document.open();
//2 x.document.write(iframe);
//2 x.document.close();
var ob = document.createElement('object');
ob.setattribute("type","application/pdf");
ob.setattribute("width","300");
ob.setattribute("height","300");
ob.setattribute("data",pdf_dataurl);
buttons.insertBefore(ob,buttons.firstChild);
e.preventDefault();
  });

  document.getElementById("edit").addEventListener("click", function(e) {
buttons.parentNode.getElementsByTagName("div")[0].contentEditable="true";
document.getElementById("open_pdf_btn").style.display="inline";
document.getElementById("compile").style.display="inline";
document.getElementById("overleaf").style.display="none";
document.getElementById("edit").style.display="none";
document.getElementById("texliverun").style.display="block";

  });

  //var pdftex_preload = new PDFTeX("pdftex-worker.js");
  pdftex_preload = undefined;
</script>
