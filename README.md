# Resume-Parsing



# Project Title


HireAnalytic.com is a job portal that I had created for people to find data science jobs all around the world. I had to shut it down since it required a lot of effort to maintain it.

To make the candidate selection process easier, I came up with a candidate recommending system which required resumes to be parsed to obtain a resume score. You can read more about it here: Candidate Recommending System . 







In hindsight, I realized that it can be used for two things


1. Parsing resumes for recruiters and adopting it to their own models

2. Beating Job portals like Indeed.com, naukri.com that parse your resume and rate you based on a score.

Here's how you can build one yourself:


## Installing 'the' libraries

There is a high chance these libraries are not installed in your notebook. So go ahead and install them.


```python
!pip install --user https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-2.3.1/en_core_web_sm-2.3.1.tar.gz

!pip install --user pyresparser

!pip install --user nltk

!python -m spacy download en_core_web_sm --user
nltk.download('stopwords')

!pip install --user python-docx
```

Here's just a gist of what each library is used for :


i. spaCy is a free, open-source library for NLP in Python. It's written in Cython and is designed to build information extraction or natural language understanding systems.

It extracts words from PDFs and DOCXs


ii. The Natural Language Toolkit (NLTK) is a platform used for building Python programs that work with human language data for applying in statistical natural language processing (NLP). It contains text processing libraries for tokenization, parsing, classification, stemming, tagging and semantic reasoning.


iii.PythonDocx is used to convert string to a word document.


PyrseParser is a simple resume parser used for extracting information from resumes. It extracts certain keywords that are most commonly found in resumes. It requires spaCy and NLTK to be installed for it work.


## Parsing a single resume

We store our resume in a variable as follows:


```python
from pyresparser import ResumeParser
k="C:/Users/Srikar/Desktop/Candidate-Recommendation System/Srikar_resume.pdf"
```
## Extracting the data from the resume using ResumeParser



```
data = ResumeParser(k).get_extracted_data()
print(data)
```




With the help of the library, we are able to extract ['name', 'email', 'mobile number', 'skills', 'college name', 'degree', 'designation', 'experience', 'company names', 'no of pages' and 'total experience'.



data.keys()




## Parsing the job description

1. Storing the job-description as a docx file

We have to store the JD as a string and then convert it to a docx file


```
#Importing the library to convert string to docx
from docx import Document

#Creating the function to convert
document = Document()

n="""With a startup spirit and 90,000+ curious and courageous minds, we have the expertise to go deep with the world’s biggest brands—and we have fun doing it. Now, we’re calling all you rule-breakers and risk-takers who see the world differently and are bold enough to reinvent it......."""

# """ - triple quotes have been used to merge several strings as a paragraph

#Adding paragraph to an empty document
p = document.add_paragraph(n)

#Saving the document as Word File
document.save('demo.docx')
```
Remember, the docx is saved in the same folder as the python file.


2. Parsing the job description document

Just like we parsed the resume, we parse the docx file as well.


```
j="C:/Users/Srikar/Desktop/andidate-Recommendation System/demo.docx"
jd = ResumeParser(j).get_extracted_data()
print(jd)
```


## Converting values into a set

We convert the the variables, 'jd' and 'data' into sets so that none of the words repeat.


```
#Extracting important data from the resume
resume_list=data['skills']+data['degree']+data['designation']+data['experience']+[data['total_experience']]

#Extracting important data from the job description
jd_list=jd['skills']+jd['designation']+[jd['total_experience']]

#Converting the list into a set
res=set(resume_list)
jd=set(jd_list)
```

## Measuring the match of resume and job description

We use the formula :

$$\frac{\left(\text{Number of unique words in resume}\cap \text{no of uniques words in JD}  \right)*100}{\text{no of uniques words in JD}}$$


Usually a good resume score is 75% or above and hence this is not the ideal resume score. You can either apply somewhere else or add the necessary keywords to match your resume :) 


You can see this Jupyter book to know how to iterate through multiple files and parse them: Link 


