# How to generate and Print  Barcode for Serial No in ERPNext??

# Tech Used

1. ## Python (> 3.5)
2. ## Python Barcode Library
```
pip install python-barcode
```
3. ## HTML Script to generate barcode sheets.
4. ## Python library to convert html page to pdf file.
```
pip install pdfkit
```
5. ## Python Library to attach all the pdfs to one. 
```
pip install PyPDF2
```
6. ## wkhtmltopdf and PIL are already installed with ERPNext.

# Steps to generate barcode PDF:

1. Generate single barcodes.
```
import barcode
from barcode.writer import ImageWriter
barcode = barcode.codex.Code128(sr_no, writer=ImageWriter())
product_img = barcode.render()
```
2.  Add Text to the Image.
```
draw = ImageDraw.Draw(product_img)
draw.text((width_to_start, height_to_start), text, (0, 0, 0), font=font)
```
3. Save the Image to the required format.
```
product_img.save("name.png", "PNG")
```
4. HTML Script to stitch the barcodes and resizing the same. Do not try to reduce the size with python library, it will erase the information stored in the barcode.

5. Convert HTML to PDF.
```
import pdfkit
# Option is page resolution
options = {
        'page-size': 'A4',
        'margin-top': '0in',
        'margin-right': '0in',
        'margin-bottom': '0in',
        'margin-left': '0in',
    }
    pdfkit.from_file('html_script.html','file_name.pdf', options=options)
```
6. Attach the single pdfs together.
```
from PyPDF2 import PdfFileReader, PdfFileWriter
pdf_writer = PdfFileWriter()
for pdf in pdfs:
    pdf_reader = PdfFileReader(pdf)
    pdf_writer.addPage(pdf_reader.getPage(0))

with open(output.pdf, 'wb') as out:
        pdf_writer.write(out)
```

# **Add as Attachment**
1. Add a validation with ```on_submit``` trigger in hooks.py.
2. Add a validation function in validations.py to call the API to generate barcode pdf but instead of downloading the file it has to be saved in the following location.
```
location = "/home/frappe/frappe-bench/sites/site_name/public/files/file_name.pdf"
```
3. Give the following location in the doctype.
```
doc.barcode_pdf = f"/files/file_name.pdf"
doc.save()
frappe.db.commit()
```
