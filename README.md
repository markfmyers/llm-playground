# llm-playground

Using the green "Use this template" button on the top right, create a copy of this repository under your GitHub account, giving it the same name. Open that repository in a codespace, then run the following commands in the terminal:

### Setup

```bash
bash setup.sh
```

That will install llm, a Python tool for interacting with different LLMs, llm-groq, which specifically connects to Groq, and llm-gemini, which connects to Google's models. You will need to generate an API key at https://console.groq.com/keys. Copy that key, then do the following command in the terminal:

```bash
llm keys set groq
```

You will paste your copied key in the terminal (it won't appear, and you might get a small popup with the word "paste" in grey. Wait a couple of seconds and you'll be able to click on it.). To make sure it worked, run this command in the terminal:

```bash
llm -m groq-llama-3.3-70b "10 names for a pet aardvark"
```

Now create an API key for Gemini here: https://aistudio.google.com/apikey. Copy that key, then do the following command in the terminal:

```bash
llm keys set gemini
```

You will paste your copied key in the terminal (it won't appear, and you might get a small popup with the word "paste" in grey. Wait a couple of seconds and you'll be able to click on it.). To make sure it worked, run this command in the terminal:

```bash
llm -m gemini-2.5-flash "10 names for a pet turtle"
```

You can see all of the models you have access to using llm with the following command:

```bash
llm models
```

### Text Models

The llm library allows us to chat with LLMs, but also to pass things to those LLMs _and_ to log our conversations in a SQLITE database. Here's one way we can start: asking it to summarize the specific contents of a web page that we'll fetch using a tool called newspaper4k that allows us to reduce the amount of tokens we send to the LLM (they have limits):

```bash
python -m newspaper --url=https://cnsmaryland.org/2025/08/02/in-the-deep-woods-a-little-owl-teaches-big-lessons/ -of=text | llm -m groq/openai/gpt-oss-120b "summarize this story in 3 paragraphs"
```

Read the story: https://cnsmaryland.org/2025/08/02/in-the-deep-woods-a-little-owl-teaches-big-lessons/ and then compare that to the summary. Is the summary a good description? Why or why not?

PUT YOUR ANSWER HERE: The summary is a good description because it breaks down the structure of the article and clearly defines the characters and the purpose they serve for the story. It initially explains Susan Hengeveld and what she is doing with these owls, before expanding on her authority/expertise, and then explains the greater significance/results of Hengeveld's work. It compresses a good story, which means it eliminates the characterization in an effort to more blandly summarize, but the point of the story still comes across.

Now it's your turn: find a story that you wrote or are very familiar with that's published online, and try the same process, altering the command and then evaluating the result.

```bash
python -m newspaper --url=https://www.nytimes.com/athletic/6692318/2025/10/05/ravens-loss-john-harbaugh-texans/ -of=text | llm -m groq/openai/gpt-oss-120b "summarize this story in 3 paragraphs"
```

YOUR EVALUATION HERE: This summary is also good. It includes every important perspective given from the players and coach Harbaugh. The magnitude of the defeat is made clear, along with the disastrous start to the season. It eliminates the context of some quotes, particularly Kyle Van Noy's, but not to a degree where the player has been misrepresented.

### Restructuring Information

Text models can do more than summarize; they can restructure information to make it more useful. We'll take a recent document describing sanctions of Maryland attorneys (the sanctionsfy25.txt file in this repository) and send it to Llama 3.3 using the `cat` command, asking the LLM to produce JSON data of some of the contents.

```bash
cat sanctionsfy25.txt | llm -m groq-llama-3.3-70b "produce only an array of JSON objects based on the text with the following keys: name, sanction, date, description. The date should be in the yyyy-mm-dd format. No yapping." 
```

That "no yapping" thing? That's one of the ways you get LLMs to stop narrating their output and just give you what you specify.

Now it's your turn: find a short (3 pages or less) news story that contains unstructured text, save it as a text file and drop it into the list of files on the left side. Then create a prompt like the one above that turns some elements of it into structured data. Put your prompt in the space below, and below that tell me how the LLM did.

```bash
cat arthurjones.txt | llm -m groq-llama-3.3-70b "produce only an array of JSON objects based on the text with the following keys: name, sanction, date, description. The date should be in the yyyy-mm-dd format. No yapping." 
```

PUT YOUR EVALUATION HERE: The results accurately depict the key moments of his life (and career), branching from when he entered the NFL, to when he signed a big contract, all the way until he sadly passed away. Considering the text itself was brief, the LLM did a good job because it did not leave out any data.

### Vision Models

One of the options we have with Groq is to use a "vision" model that can describe the information in certain types of images. Let's test that out using an image from this Maryland physician discipline report: https://www.mbp.state.md.us/BPQAPP/orders/D002592202.265.pdf. The specific vision model we're using only accepts jpg, gif and png files, so I've created a PNG from the first page. Run this command in the terminal:

```bash
llm -m gemini-2.5-flash "what is the license number from this image?" -a md_doc.png
```

You should get something like: "The license number in the image is D25922." as a response.

Now it's your turn: find an image _that you would be ok showing to your grandmother_ and make sure it's a PNG, GIF or JPG file. Drag it into the codespace so you can see it in your list of files, then change the command below so that it refers to your image and asks a different question. Edit the README to do that so I can see your work. How did the LLM do?

```bash
llm -m gemini-2.5-flash "what is the id number from this image" -a idfront.jpg 
```

PUT YOUR EVALUATION HERE: The LLM correctly answered the question.
The ID number from the image is **120992513**.


### Audio Models

Another option is to use models that transcribe audio. Listen to [this mp3 file](https://dare.wisc.edu/audio/south-carolina-desegregating-edisto-state-park/) from the Dictionary of American Regional English project at the University of Wisconsin. Click the download link and save the .mp3 file to your computer, then drag it to the file to the list of files on the left. Then, using llm, have the `whisper-large-v3-turbo` model from Groq provide a transcription using that file name.

```bash
llm -m groq/whisper-large-v3-turbo -a audio.mp3 
```

How did the LLM do compared to the original transcript?
**Groq transcription** Another thing that we encountered during this period was we had in Charleston the Edge Door State Park, which was also operated on a lily-white basis while tax monies paid for its operations. We again went into the federal courts and prior to going into the court, the state of South Carolina decided that they would close the state park down. Therefore, when we reached the court several days later, it was looked upon as a new case. However, the members of the black community did not feel too badly about it because they They were satisfied with the fact that the golf course was coming in. The state park was closed down to all persons and that no one could enjoy any of the facilities. This was a tragic situation to see such a beautiful tract of land with such wonderful buildings on such a beautiful strand of ocean closed down because of the segregated thinking of many of our lawmakers.

PUT YOUR EVALUATION HERE: The LLM made a few critical errors. It did not transcribe moments where the speaker misspoke and corrected himself, so it loses context. For example, the first time the speaker mentions the state park, he initially says golf course before correcting himself. That is absent from the LLM's transcription. Later on, the LLM misquoted 'moot' to be 'new,' which makes a big difference. The model again repeated its error of ignoring times the speaker corrected himself, when it transcribed the sentences about the members of the black community. Overall, the LLM did a poor job because it made multiple mistakes that change the interpretation of the transcript, as well as the tone of the speaker.

### How You Could Use AI

Thinking about news archives, write a few examples of how you might use LLMs to help (beyond writing code). Be specific: don't say that you'll use it to accomplish a larger goal. Instead, say how it could perform or improve specific tasks. I encourage you to think big.

LLM's can help to sift through large data sources. They are capable of inputting specific commands and extracting specific datapoints, such as data from a specific year or data which includes a specific word or phrase. Additionally, LLM's can identify data in various multimedia forms. Although the whisper model did not do the greatest job, it is important to know an LLM can be used to decipher visual, audio, and written data. I can use LLM's to analyze photos, like I did for this task, as well as to notice trends between multiple documents or groups of data.
### Finishing Up

When you are finished, add, commit and push your work to GitHub and submit the URL of the repository in ELMS.
