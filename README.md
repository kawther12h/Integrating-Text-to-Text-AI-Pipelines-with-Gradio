# Integrating-Text-to-Text-AI-Pipelines-with-Gradio

# Text Summarization

## Main Task
Using Gradio and Hugging Face pipelines specifically for text-summarization tasks.


##  purpose of the project.

Reading a long web artical may consume time and effort, therefore you need a quick tool to quickly summarise the information.
The purpose of this project is to provide a straightforward and user-friendly web-based application interface for text summarisation by utilising Hugging Face's text summarisation model and the Gradio library's capabilities to create the interface.

## Hugging Face text-to-text pipeline
Here we use ("facebook/bart-large-cnn") as  summarization Model
[Text_Summarization_Pipeline](https://github.com/kawther12h/Integrating-Text-to-Text-AI-Pipelines-with-Gradio/blob/main/HuggingFace_pipeline_codeipynb.ipynb)

## Gradio Interface
Here the Gradio components That used in the project interface
[Gradio_Interface](https://github.com/kawther12h/Integrating-Text-to-Text-AI-Pipelines-with-Gradio/blob/main/Gradio_Code.ipynb)

## Full Building Project
Here the full Hugging Face text-to-text
pipeline integrated with Gradio for summarizing webpage artical.
[summarizing webpage artical](https://github.com/kawther12h/Integrating-Text-to-Text-AI-Pipelines-with-Gradio/blob/main/Artical_Text_Summarization_Assi_1.ipynb)
## How the Hugging Face text-to-text pipeline works.

### Hugging Face text-to-text pipeline is used to perform text summarization. Here's a breakdown of how it works:
* Install and import the needed library for loding pre-trained model, requst and retrive webpage content and parsed HTML document to navigate and extract information from HTML webpage

* Load the Summarization Model: The pre-trained BART model, which is specifically designed for text summarization tasks
```python
 pipeline("summarization", model="facebook/bart-large-cnn")
``` 

* Get the webpage from URL and extract the text content of the webpage. Find all h1 ,h2 and p tags in webpage then stores them in list. Finally, joins all the extracted text into a single string.
```python
URL = "https://hackernoon.com/will-the-game-stop-with-gamestop-or-is-this-just-the-beginning-2j1x32aa"

# Request the URL and retrieve the web page content
r = requests.get(URL)

# Object from BeautifulSoup to extract the text content of the webpage, parsing the HTML content
soup = BeautifulSoup(r.text, 'html.parser')

# To finds all the <h1> (header) and <p> (paragraph) elements in the HTML content
results = soup.find_all(['h1','h2' ,'p'])

# Extract the text content from each element and store it in a list called text
text = [result.text for result in results]

#This joins all the extracted text into a single string, representing the entire article.
ARTICLE = ' '.join(text)
```

* Replace sentence-ending punctuation with a special token (<eos>) . This helps split the article into smaller list chunks for summarization.
```python
ARTICLE = ARTICLE.replace('.', '.<eos>')
ARTICLE = ARTICLE.replace('?', '?<eos>')
ARTICLE = ARTICLE.replace('!', '!<eos>')

sentences = ARTICLE.split('<eos>')
```
* For loop iterates through each sentence in the sentences list:

If the length of the current chunk (in terms of words) plus the length of the current sentence (split by spaces) is less than or equal to the max_chunk length:
The sentence is added to the current chunk.

Otherwise:

The current_chunk index is incremented to move to the next chunk.
A new chunk is created, and the current sentence becomes the first sentence in this new chunk.

```python
for sentence in sentences:
    if len(chunks) == current_chunk + 1:
        if len(chunks[current_chunk]) + len(sentence.split(' ')) <= max_chunk:
            chunks[current_chunk].extend(sentence.split(' '))
        else:
            current_chunk += 1
            chunks.append(sentence.split(' '))
    else:
        print(current_chunk)
        chunks.append(sentence.split(' '))
```
* Finally, the loop iterates through each chunk,
to ensures that each chunk is represented as a single string (rather than a list of words)
```python
for chunk_id in range(len(chunks)):
    chunks[chunk_id] = ' '.join(chunks[chunk_id])
```
* Apply Summarization to text with lenth of 30-120 word for each chunk

```python
res = summarizer(chunks, max_length=120, min_length=30, do_sample=False)
```

* Extracting the 'summary_text' value from each summary
```pyhton
text = ' '.join([summ['summary_text'] for summ in res])
```



## Usage

### Instructions on how to run the code
* provide URL of webpage.
* djust Minimum and Maximum Length of summarization.
* Send the requst.

## The expected output
![Screenshot 2024-09-14 091448](https://github.com/user-attachments/assets/b9c7dae1-6879-4526-aa1d-4904709db943)

## Hugging Face project page
[Hugging Face project space](https://huggingface.co/spaces/Kawthar12h/Text_Summarization)
## Explaination Video

Vidio
