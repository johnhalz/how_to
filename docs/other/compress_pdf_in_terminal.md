# How to Compress a PDF in Terminal

Open the terminal and go to the directory where the original PDF is located. For this example, let us say that the name of the PDF in question is called `original.pdf`.

Type in the command:

```bash
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/prepress -dNOPAUSE -dQUIET -dBATCH -sOutputFile=original_compressed.pdf original.pdf
```

And you should have the new file called `original_compressed.pdf` in the same directory!


