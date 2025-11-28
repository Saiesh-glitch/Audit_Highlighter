Approach Explained

For this assignment, I created a Python program in Google Colab that takes two inputs from the user:

A file upload (PDF, Excel, Word, or Image)

A text or phrase that the user wants to search for

The main objective is to locate all occurrences of the phrase in the document and draw a red, unfilled bounding box around them, without changing the original file. A new highlighted copy is generated instead.

How the program works

User uploads a document
Using files.upload() from Google Colab, the user can upload any supported file type.

Detect the file type
The program checks the file extension.
Based on that, it chooses the correct processing method:

If the file is a PDF, it is processed directly.

If it is Word (.doc/.docx) or Excel (.xls/.xlsx), it is first converted to PDF using LibreOffice.

If it is an image (.png/.jpg/.jpeg), it is processed through OCR.

Convert Word/Excel to PDF
I used LibreOffice in headless mode (soffice --convert-to pdf) because it preserves page formatting and makes text searchable.

Search for the phrase
For PDFs, I used PyMuPDF (fitz) to read the text at the word level, which also gives the exact coordinates of each word.
For images, I used Tesseract OCR to extract words with their bounding box positions.

Match the phrase, not each word separately
Instead of highlighting every word individually, the program splits the phrase into words and looks for them in sequence using a sliding-window approach.
When a full phrase is found, it combines the bounding boxes of all the words and creates one single box around the whole phrase.

Draw bounding box

In PDFs: I added an annotation rectangle using page.add_rect_annot(), set the border color to red, and no fill.

In images: I used Pillow to draw a red rectangle outline.

Save output without modifying the original
A new file is saved with _highlighted added to the name.
Example: document.pdf becomes document_highlighted.pdf.
