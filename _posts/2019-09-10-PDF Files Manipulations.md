---
layout: post
title: 10. PDF FILES Manipulations
category: Python
tag: Python
---
- Please install PyPDF2 which is a common python library to work with PDF files.
- The library can be used to read text from PDF files
- You can install the library as follows: pip install PyPDF2

# Extract information From A PDF

```python
from PyPDF2 import PdfFileWriter, PdfFileReader
#this is the module how to write or reader in python
# two class we need
# similar to CSV files operation
# Read it as a binary files
f = open('Harvard_Business_School.pdf', 'rb')
# rb is read binary means
# Create a PDF reader object
read_pdf = PdfFileReader(f)
# obtain some document information
read_pdf.getDocumentInfo()
```

```
{'/Author': 'Barlow, Andrew Jonathan',
 '/Company': 'Harvard University',
 '/CreationDate': "D:20180817171357-04'00'",
 '/Creator': 'Acrobat PDFMaker 18 for Word',
 '/ModDate': "D:20180817171437-04'00'",
 '/Producer': 'Adobe PDF Library 15.0',
 '/SourceModified': 'D:20180817211351',
 '/Title': 'I'}
```

```python
# apply the Number of pages attribute method to get the number of pages
read_pdf.numPages
# how many page we have read_pdf.numPages
# 8
```

```python
# Grab any page
sample_page_text = read_pdf.getPage(2).extractText()
# extracttext out of page
# first page is it started from '0' in python
```

```
'IS \nBUSINESS \nSCHOOL \nRIGHT FOR \nYOU?\n Graduates of \nMBA \nprograms can be found in almost any type of organization. Business school will \nprepare you to create or lead an organization, manage resources, develop effective operational \nstrategies, and more. \nOnce \nadmitted, r\nequired coursework typically include\ns: Organizational \nBehavior, Marketing, Accounting, Finance, Strategy\n, \nand Operations Management. This is followed \nby elective coursework that allows the student to customize their experience. Some students \n\ncon\nsider an MBA as essential for advancement to a management role while others will use it as a \n\nmeans to change careers. As an undergraduate student, it is unlikely that you will be admitted to \n\nenter directly into an MBA program without first working for a fe\nw years. This period of \n\nemployment will give you time to think about your long term goals and help you determine if a \n\ngraduate degree is appropriate.\n Informational \nmeetings\n Talking with\n people (with or without MBAs) currently working in\n field\ns that inter\nest you\n can be \nextremely helpful as you consider your \nfuture career \noptions.\n Be sure to ask:\n How is an \nMBA viewed by the industry\n?Will an MBA improve your chances of being hired and moving ahead\n?If that person has an MBA, ask about their experience in the program they attended\nWhat was most helpful for you as you prepared your application?\nYour conversation can help you put into context how applicable the program will be to your career \n\ngoals. Also, speak with current students or those who have recently \ncompleted an MBA program. \nInquire about their experience in the program and how it has i\nmpact\ned their careers.\n What should you do to prepare for business school?\n There™s no magic formula for\n the perfect undergraduate conce\nntration\n, set of academic \nachievements, pre\n-MBA work experience\n, \nextracurricular accomplishments, essays\n, interviews, or \nrecommendation\ns. Nor is there a common \n\ncareer goal or industry in which you can \n\naspire to work. What MBAs have in common is \n\nthey find opportunities to take initiative, they \n\nare motivated, have a strong awareness of \n\nthemselves, and have a desire to learn and \n\ngrow.\n  \nBusiness schools tend to focus on impact \nmore than scale. Make sure, therefore, that the \n\nacademic and extracurricular choices you \n\nmake truly reflect your interests, demonstrate initiative, and give you opportunities to play a \n\nleadership role in the organization. A strong undergraduate academic record and GMAT (or \n\npossibly GRE) score are\n also\n important parts of your a\npplication to business school. Many sc\nhools \nallow you to sit in on a class to gain insight into what a typical course looks like. Take advantage of \n\nthis opportunity!\n Business schools look for leaders. In \nwhat ways have you demonstrated \ninitiative or translated a vision into a \n\nreality? Bear in mind that it™s not about \n\nthe size of your accomplishment, but the \npassion with which you invest in and \npursue your goals. \n'
```

```python
f.close()
```

# Copy A single page and put it in a newly created PDF

```python
# Read a sample page
f = open('Harvard_Business_School.pdf', 'rb')
read_pdf = PdfFileReader(f)
sample_page = read_pdf.getPage(4) # we got page
```

```python
# Create a writer object
write_pdf = PdfFileWriter()
write_pdf.addPage(sample_page)
#write_pdf = PdfFileWriter() this mean write_pdf is nothing.
#write_pdf.addPage(sample_page) means that input selected page into nothing
```

```python
# open a newly created file
pdf_output = open('Harvard_New.pdf', 'wb') #writing porpuse like create new file
write_pdf.write(pdf_output) # write_pdf contento input created from pdf_output
pdf_output.close() # Close the new file
f.close() # close the source file
```

# ROTATE PDFS AND WRITE THEM TO A NEW PDF

```python
# Open the Source file
f = open('Harvard_Business_School.pdf', 'rb')
read_pdf = PdfFileReader(f)

# Create a writer object
write_pdf = PdfFileWriter() # we written anything yet

# Read a page and rotate it
rotated_page = read_pdf.getPage(2).rotateClockwise(90)

# Add the rotated page to the writer object
# PLEASE DO NOT FOREGET THAT 'P' is uppercase!
write_pdf.addPage(rotated_page)

# Save the writer object somewhere!
pdf_output = open('Harvard_New_rotated.pdf', 'wb')
write_pdf.write(pdf_output)
pdf_output.close() # Close the new file
f.close() # Close the new file

```

# READ MULTIPLE PAGES
```python
f = open('Harvard_Business_School.pdf', 'rb')
read_pdf = PdfFileReader(f)

pdf_text_all = [] # Create an empty list to hold the data
num_pages = read_pdf.numPages # this is how many page they are obtained

num_pages # 8
```

```python
for page in range(num_pages): # range(8) means that to make (0,1,2,3,4,5,6,7) like number step by step
    one_page_text = read_pdf.getPage(page).extractText()
    pdf_text_all.append(one_page_text)
len(pdf_text_all)
# 8
pdf_text_all # Every single page summarized in a list!
```

```
'Undergraduate Resource Series\nO˜ce of Career Services | 54 Dunster Street     \nHarvard University | Faculty of Arts and Sciences | 617.495.2595\nwww.ocs.fas.harvard.edu\nOCSAPPLYING  TO \nBUSINESS  SCHOOLPhoto: Harvard University News O˜ce\n',
 '© 201 President and Fellows of Harvard CollegeAll rights reserved.\nNo part \nof this publication may be reproduc\ned in any wa\ny without the express written \npermission of the Harvard University Faculty of Arts & Sciences Office of Career Services.08/1O˜ce of Career Services\nHarvard University\n\nFaculty of Arts & Sciences\n\nCambridge, MA 02138\n\nPhone: (617) 495-2595\n\nwww.ocs.fas.harvard.edu\n',
 'IS \nBUSINESS \nSCHOOL \nRIGHT FOR \nYOU?\n Graduates of \nMBA \nprograms can be found in almost any type of organization. Business school will \nprepare you to create or lead an organization, manage resources, develop effective operational \nstrategies, and more. \nOnce \nadmitted, r\nequired coursework typically include\ns: Organizational \nBehavior, Marketing, Accounting, Finance, Strategy\n, \nand Operations Management. This is followed \nby elective coursework that allows the student to customize their experience. Some students \n\ncon\nsider an MBA as essential for advancement to a management role while others will use it as a \n\nmeans to change careers. As an undergraduate student, it is unlikely that you will be admitted to \n\nenter directly into an MBA program without first working for a fe\nw years. This period of \n\nemployment will give you time to think about your long term goals and help you determine if a \n\ngraduate degree is appropriate.\n Informational \nmeetings\n Talking with\n people (with or without MBAs) currently working in\n field\ns that inter\nest you\n can be \nextremely helpful as you consider your \nfuture career \noptions.\n Be sure to ask:\n How is an \nMBA viewed by the industry\n?Will an MBA improve your chances of being hired and moving ahead\n?If that person has an MBA, ask about their experience in the program they attended\nWhat was most helpful for you as you prepared your application?\nYour conversation can help you put into context how applicable the program will be to your career \n\ngoals. Also, speak with current students or those who have rec
```

# WaterMark to PDF
```python
from PyPDF2 import PdfFileWriter, PdfFileReader

# Open watermark
f = open('watermark_conf_2.pdf', 'rb')
read_watermark = PdfFileReader(f)
watermark_page = read_watermark.getPage(0)

# Open file to be watermarked
f = open('Harvard_Business_School.pdf', 'rb')
read_pdf = PdfFileReader(f)


# Create a writer object
write_pdf = PdfFileWriter()

# watermark all pages
num_pages = read_pdf.getNumPages()
num_pages
# 8
```

```python
for page in range(num_pages):
    single_page = read_pdf.getPage(page)
    single_page.mergePage(watermark_page) # merge watermark_page into read_pdf.file
    write_pdf.addPage(single_page)


# Save the writer object somewhere!
pdf_output = open('Harvard_watermarked.pdf', 'wb')
#wb is the writing funcition.
write_pdf.write(pdf_output)
pdf_output.close() # Close the new file
f.close() # Close the new file    
```
