# Resume Reader Assignment
## Abstract
The code reads pdf files and extracts required sections for the document (education, experience, skills). Then a predefined LLM is used to generate questions and grade answers.
## Libraries used 
* PyMuPDF 
* Pandas 
* re
* Numpy
* Fuzzywuzzy
* Datetime
* Dateutil
* Google.generativeai
* Unidecode

## Working of the System 
The System works on readable PDF files. Pymupdf is used to read uploaded PDF files:-

1. Then the document information is exctracted in the format and use of Pymupdf dict, then blocks, then lines, and finally the spans of each document. The extracted informaiton for each span includes font size, font type, and span text. This will help distinguish headers from regular texts this is done through examining font size, all caps in the text, and if the text is bold. All this information is cleaned using re and unidecode and stored in span_df dataframe.
2. After that each span (text) is given a span score stored in span_scores which is used to find the span scores (values), and the frequency of each value. The most occurring value is found, this will help distinguish regular texts from headers. The idea behind this is that regular texts appears more often than headers. Then each span score is given a tag, p for the most occurring score, hx for values higher than p and hx help find header levels with the highest score for h1 and so on (Keep in mind that span_scores is order descendingly based on values), and finally sx for values (scores) smaller tham p.
3. The next step is turn the document into a tree like structure this will help in extracting sections and finding the content of each header (h1 contents h2s underneath, h2s contents h3s, and so on) this makes more efficient in extracting specific section based on headers. First headers are identifed and stored separately with the texts of tags p and s stored in the header content above them. After headers are identified the next step will be storing lower header levels inside higher levels above them.
4. After building the header structure, a recursive function is used to search and extract the content of any requested header. The required headers are identified using a list of common words for each header (skills, education, and experience). Fuzzywuzzy will be used to compare and find the header with the highest probability and using the recursive function to find the header and its content.
5. Then using the work experience section to find dates using re and calculate the candidate's years of experience using datetime and dateutil libraries.
6. The Skills section is cleaned and sent with the prompt for google gemini to generate question and later grade answers.
7. Finally, everything is written on to the CV_Report.txt as the report of the CV.

## Files
1. CV.pdf: the resume used as an input.
2. cvreader_code.ipynb: code 
3. CV_Report.txt: the output
