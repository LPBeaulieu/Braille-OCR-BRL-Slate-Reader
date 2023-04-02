# BRL-Slate-Reader
This braille OCR application can convert JPEG braille text images into RTF documents, while removing typos for you!

![Thumbnail](https://github.com/LPBeaulieu/Braille-OCR-BRL-Slate-Reader/blob/main/Github%20Page%20Images/BRL-Slate-Reader%20Thumbnail.png)
<h3 align="center">BRL-Slate-Reader</h3>
<div align="center">
  
  [![License: AGPL-3.0](https://img.shields.io/badge/License-AGPLv3.0-brightgreen.svg)](https://github.com/LPBeaulieu/Braille-OCR-BRL-Slate-Reader/blob/main/LICENSE)
  [![GitHub last commit](https://img.shields.io/github/last-commit/LPBeaulieu/Braille-OCR-BRL-Slate-Reader)](https://github.com/LPBeaulieu/Braille-OCR-BRL-Slate-Reader)
  [![GitHub issues](https://img.shields.io/github/issues/LPBeaulieu/Braille-OCR-BRL-Slate-Reader)](https://github.com/LPBeaulieu/Braille-OCR-BRL-Slate-Reader)
  
</div>

---

<p align="left"> <b>BRL-Slate-Reader</b> is a tool enabling you to convert scanned braille pages (in JPEG image format and written with a Braille slate) into Braille Ready File (BRF) digitized braille and rich text format (RTF) documents, complete with formatting elements such as alignment, paragraphs, <u>underline</u>, <i>italics</i>, <b>bold</b> and <del>strikethrough</del>, basically allowing you to include any formatting encoded by RTF commands or braille typeform indicators.</p>
<p align="left"> A neat functionality of <b>BRL-Slate-Reader</b> is that the typos (sequence of at least two successive full braille cells)
  automatically get filtered out, and do not appear in the final RTF text nor in the BRF file. The BRF file can in turn be used to print out copies of your work on a braille embosser, or to read them electronically using a refreshable braille display.
  
  - My <b>deep learning model</b> for the Perkins Brailler along with the dataset and other useful information may be found on my Google Drive at the following link: https://drive.google.com/drive/folders/1RNGUoBJOSamYOaO7ElFBeWIRVpHtlQpd?usp=sharing. The model is also available for download on this github repo. Also check out my other github repo <b>e-Braille Tales</b> for a similar OCR application with the Perkins Brailler (https://github.com/LPBeaulieu/Braille-OCR-e-Braille-Tales).
  
    <br> 
</p>

## 📝 Table of Contents
- [Dependencies / Limitations](#limitations)
- [Getting Started](#getting_started)
- [Usage](#usage)
- [Author](#author)
- [Acknowledgments](#acknowledgments)

## ⛓️ Dependencies / Limitations <a name = "limitations"></a>
- This Python project relies on the Fastai deep learning library (https://docs.fast.ai/) to generate a convoluted neural network 
  deep learning model, which allows for braille optical character recognition (OCR). It also needs OpenCV to perform image segmentation 
  (to crop the individual characters in the braille page images).
  
- When writing text on the Braille slate, unless a space is included at the end of a line or at the beginning of the next line, the last word on the line will be merged with the first characters on the next one, up to the next space. As such, the <b>"line continuation without space" braille symbol ("⠐") is not required and should be avoided</b>, as it could be confused with other braille characters, such as initial-letter contractions. However, line continuations with a space ("⠐⠐") can be used without problem in this application.

- In this application a space needs to be included after any RTF command (even though the RTF specifications state that it is an optional space). The reason for this is that when the code is transcribing the braille into printed English, it often needs to determine if any given braille character stands alone. A braille character that stands alone means that it is flanked by characters such as empty braille cells ("⠀") or dashes, but not by a       braille character mapping to a letter or number, such that can be found at the end of every RTF command. In other words, <b>you must include a space after any RTF commands</b>. Here is an example: "This requirement \strike strikes \strike0 me as being important!", which in braille would be written as follows: "⠠⠹⠀⠗⠑⠟⠥⠊⠗⠑⠰⠞⠀⠸⠡⠎⠞⠗⠊⠅⠑⠀⠎⠞⠗⠊⠅⠑⠎⠀⠸⠡⠎⠞⠗⠊⠅⠑⠼⠚⠀⠍⠑⠀⠵⠀⠆⠬⠀⠊⠍⠏⠕⠗⠞⠁⠝⠞⠖".

- Importantly, <b>the pages must be scanned in landscape mode, with the left margin placed on the flatbed scanner in such a way that the shadows produced by the scanner light will face away from the left margin</b> (the shadows will face the right margin of the page, when the page is viewed in landscape mode). This is because the non-white pixels actually result from the presence of shadows, the orientation of which plays a major role in image segmentation (determining the x and y coordinates of the individual characters) and optical character recognition (OCR). For best results, the braille document should be <b>typed on white braille paper or cardstock and scanned in landscape mode as grayscale images on a flatbed scanner at a 300 dpi resolution with the paper size setting of the scanner set to letter 8 1/2" x 11"</b>. The darkness settings of the scanner might also need to be adjusted to acheive an optimal braille shadow to noise ratio. When scanning the braille pages, care should be taken to close the scanner lid slowly, so as to avoid moving the page, which could result in misalignment and subsequent segmentation issues. <b>To ensure that the segmentation has proceeded adequately, the segmentation result image (scanned image overlaid with green character rectangles) for every scanned page of the braille document should be quickly inspected</b>. These images are generated by the code and stored in the "Page image files with rectangles" folder, which is created automatically by the code.  
  
- <b>The slate must be calibrated</b>, meaning that the number of pixels between a dot in the top left Braille cell and its corresponding dot in the top right Braille cell (in landscape mode, see Figure for more on this step). The same procedure needs to be done for the vertical dimension of the slate that is within the frame of the scanner in landscape mode. That is to say, you must provide the number of pixels between a dot in the top left corner, and the corresponding dot in the lowest cell on the left edge of the slate that fits onto the scanner in landscape mode. Additionally, the number of columns and the number of rows that will be visible on the scanner in landscape mode need to be provided. Finally, the "x" and "y" pixel coordinates of the center of the top left dot of the top left Braille cell need to be passed in when calibrating the slate. When running the code, it will provide you with the number of horizontal and vertical pixels in-between braille cells that has been calculated using the above information. However, you might want to fine-tune the calibration step by increasing or decreasing the value of "horizontal_spacer_pixels:" and "vertical_spacer_pixels:" that were calculated by the code. Simply pass in your numbers of choice after these arguments when running the code, and check the scanned image overlaid with green segmentation rectangles to see if there are any improvements. Adjust the value of these variables until the green rectangles line up nicely with the braille characters on the image. The code will then remember these optimal parameters for the calibrated slate, which are stored in the "Default_Parameters" folder, which is created automatically by the code.   

- When filling in the cells of mistakes, make sure that there are at least two consecutive full braille cells ("⠿") after correction, as otherwise a single full cell will be interpreted as "for" in the RTF document. 
 
 
## 🏁 Getting Started <a name = "getting_started"></a>

The following instructions will be provided in great detail, as they are intended for a broad audience and will
allow to run a copy of <b>BRL-Slate-Reader</b> on a local computer.

The instructions below are for Windows operating systems, and while I am not 100% sure that it is able to run on Windows, I made every effort to adapt the code so that it would be compatible, but the code runs very nicely on Linux.

Start by holding the "Shift" key while right-clicking in your working folder, then select "Open PowerShell window here" to access the PowerShell in your working folder and enter the commands described below.

<b>Step 1</b>- Install <b>PyTorch</b> (Required Fastai library to convert images into a format usable for deep learning) using the following command (or the equivalent command found at https://pytorch.org/get-started/locally/ suitable to your system):
```
pip3 install torch torchvision torchaudio
```

<b>Step 2</b>- Install the <i>CPU-only</i> version of <b>Fastai</b>, which is a deep learning Python library. The CPU-only version suffices for this application, at least when running on Linux, as the character images are very small in size:
```
py -m pip install fastai
```

<b>Step 3</b>- Install <b>OpenCV</b> (Python library for image segmentation):
```
py -m pip install opencv-python
```

<b>Step 4</b>- Create the folder "OCR Raw Data" in your working folder:
```
mkdir "OCR Raw Data" 
```

<b>Step 5</b>- You're now ready to use <b>BRL-Slate-Reader</b>! 🎉

## 🎈 Usage <a name="usage"></a>
The "BRL-Slate-Reader.py" Python code converts JPEG braille text scans into printed English in the form of a Rich Text Format (RTF) document and digitized braille as a Portable Embosser Format (PEF) file. In addition to the RTF and PEF files, the code will generate a braille text file (".txt") containing the OCR results before transcription to printed English, so that you could revisit the text in braille form. Each page of this ".txt" file will line up with the pages written on the Perkins Brailler and will be separated from one another by two carriage returns, to ensure easy navigation throughout the document. You can find instructions on how to use <b>BRL-Slate-Reader</b> on my YouTube channel: https://www.youtube.com/watch?v=U8-s8eQXInI.<br>

- In order to submit a scanned braille text page to the code, you will need to <b>place the JPEG image in the "OCR Raw Data" subfolder of your working     folder</b>, which you created at step 8 of the "Getting Started" section.

- <b>Please note that all of the JPEG file names in the "OCR Raw Data" folder must contain at least one hyphen ("-") in order for the code
  to properly create subfolders in the "OCR Predictions" folder.</b> These subfolders will contain the RTF document, along with the PEF and ".txt" braille files. The reason for this is that when you will scan a multi-page document, you will provide your scanner with a file root name (e.g. "my_text-") and the scanner will number them automatically (e.g."my_text-.jpg", "my_text-0001.jpg", "my_text-0002.jpg", "my_text-"0003.jpg", etc.) and   the code would then label the subfolder within the "OCR Predictions" folder as "my_text". The OCR prediction results for each page will be added in sequence to the "my_text.txt" file within the "my_text" subfolder of the "OCR Predictions" folder. Should you ever want to repeat the OCR prediction for a set of JPEG images, it would then be important to remove the "my_text" subfolder before running the "get_predictions.py" code once more, in order to avoid appending more text to the existing "my_text.txt" file.
  
- Then, run the "BRL-Slate-Reader.py" Python script by opening the command line from your working folder, such that you will already be in the correct path and copy and paste the following in command line:  
```
python3 BRL-Slate-Reader.py
```

- The first thing that the code will do is perform segmentation (determine the x and y coordinates of every braille character). The segmentation results are visible in the "Page image files with rectangles" folder, which is created automatically by the code. You might need to <b>adjust the value of the variable "x_min" at line 140 of the "BRL-Slate-Reader.py" Python code</b>, in order to initially calibrate the code to your Perkins Brailler/scanner combination. Remember to <b>always set the left margin of the Perkins Brailler to its minimum setting</b> (see explanation above in the                   "Dependencies / Limitations" section). Go ahead and open the JPEG file with segmentation results (green rectangles) in a photo editing software such as GIMP. Take note of the pixel at which the braille character starts along the x axis (in landscape mode) and update the value at line 140 of the "e-BRL-Slate-Reader.py" Python code. You should only need to find the pixel value of "x_min" and update it in the code once, as illustrated in Figure 1. 

![Braille Slate Calibration Instructions](https://github.com/LPBeaulieu/Braille-OCR-BRL-Slate-Reader/blob/main/Github%20Page%20Images/Slate%20Calibration%20Instructions.png)<hr>
<b>Figure 1</b>: The pixel along the x-axis (in landscape mode) at which segmentation should start on every line can be found by opening the scanned braille JPEG image in a photo editing software such as GIMP and locating the pixel closest to the left margin (see red arrows), here "x_min" is set to 282 pixels.

![Finding the pixel coordinates in GIMP](https://github.com/LPBeaulieu/Braille-OCR-BRL-Slate-Reader/blob/main/Github%20Page%20Images/Finding%20the%20pixel%20coordinates%20in%20GIMP.png)<hr>
<b>Figure 1</b>: The pixel along the x-axis (in landscape mode) at which segmentation should start on every line can be found by opening the scanned braille JPEG image in a photo editing software such as GIMP and locating the pixel closest to the left margin (see red arrows), here "x_min" is set to 282 pixels.

- Alternatively, it is possible to resubmit the text (".txt") file to the "e-braille-tales.py" Python code once you have made modifications to it. The braille text will be extracted from the ".txt" file and the carriage returns that were introduced to facilitate proofreading will be automatically removed by the code, if still present. Simply place the corrected ".txt" file in the "OCR Raw Data" subfolder of your working folder and include the name of your text file when running the Python code, as follows:
```
python3 e-braille-tales.py "my_text_file_name.txt"
```
- When providing Python with the name of your file (and placing the text file in the "OCR Raw Data" folder), the OCR step will be circumvented and your braille text will be converted to the RTF and PEF files. You can continue this process until all mistakes have been dealt with.
 
- The following RTF commands are automatically converted into PEF tags by the code and are transcribed from braille to English RTF commands in the RTF file: 

  - The braille equivalent of the tab RTF command "\tab" ("⠸⠡⠞⠁⠃") will be changed to two successive empty braille cells ("⠀⠀").
  - A line break RTF command "\line" ("⠸⠡⠇⠔⠑") will be converted into a line break (\</row>\<row> PEF tags).
  - New paragraph RTF commands "\par" ("⠸⠡⠏⠜") will be mapped to a line break (\</row>\<row> PEF tags) followed by two successive empty braille cells, as new paragraphs in braille documents are typically started by two empty braille cells that serve as a tab. Similarly, in the RTF document, any braille new paragraph RTF commands "\par" ("⠸⠡⠏⠜") will be switched to "\par \tab" to add a tab at the start of every new paragraph.
  - The page break RTF commands "\page" ("⠸⠡⠏⠁⠛⠑") are changed for a line break, followed by a page break (\</row>\</page>\<page>\<row> PEF tags).
  - New section RTF commands "\sbkpage" ("⠸⠡⠎⠃⠅⠏⠁⠛⠑") are swapped out for a line break, followed by a page and section break 
    (\</row>\</page>\</section>\<section>\<page>\<row> PEF tags).

  For an in-depth explanation of all the most common RTF commands and escapes, please consult: https://www.oreilly.com/library/view/rtf-pocket-guide/9781449302047/ch01.html.

  These are the only RTF commands that are automatically removed from the braille text and converted into PEF tags. All other RTF commands (if present) will be carried over in braille form into the PEF file and could be removed manually afterwards. However, as braille already encompasses typeform  indicators for symbols, words and passages written in caps, italics, bold, underline or script (font size of 28), as well as symbols in superscript or   subscript, there should be limited need to resort to other RTF commands than those listed above. 
  
- When using grade I ("⠰") or numeric ("⠼") indicators, these should be placed directly in front of the characters they will be affecting. The next order of priority is the capitalization indicators ("⠠"), followed by the other typeform indicators (bold, italics, underline, script) and finally by superscript "⠰⠔" or subscript "⠰⠢" indicators. 
 
     
<br><b>And that's it!</b> You're now ready to convert your braille manuscript into digital format! If you are close to someone who is visually impaired and would like to help them find meaningful work through technology, or maybe if you are only sprucing up your braille skills in preparation for the Zombie Apocalypse (lol) then this app is for you! 🎉📖
  
  
## ✍️ Authors <a name = "author"></a>
- 👋 Hi, I’m Louis-Philippe!
- 👀 I’m interested in natural language processing (NLP) and anything to do with words, really! 📝
- 🌱 I’m currently reading about deep learning (and reviewing the underlying math involved in coding such applications 🧮😕)
- 📫 How to reach me: By e-mail! LPBeaulieu@gmail.com 💻


## 🎉 Acknowledgments <a name = "acknowledgments"></a>
- Hat tip to [@kylelobo](https://github.com/kylelobo) for the GitHub README template!




<!---
LPBeaulieu/LPBeaulieu is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

