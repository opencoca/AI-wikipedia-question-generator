# Wikipedia Question Generator

The Wikipedia Question Generator is a unique open-source project maintained by 12787326 Canada Inc. This project leverages Natural Language Processing (NLP) and Wikipedia content to generate interesting Mad Libs-style game questions, making it a fun and educational tool. It is open source under the [AGPL License](LICENSE).

The project was originally built for the [TrackMaven Monthly Challenge meetup](http://www.meetup.com/TrackMaven-Monthly-Challenge/events/218683569/) in December 2014, and has since grown in popularity due to its versatility and educational benefits. You can get an idea of the type of game this tool supports by watching [this YouTube video](https://www.youtube.com/watch?v=UJR1BYjtI7c&feature=youtu.be&t=1m46s).

A live version of the application can be accessed at [wikitrivia.atbaker.me](http://wikitrivia.atbaker.me).

## Sample Usage

You can easily generate trivia questions using the Wikipedia Question Generator. For example, the following command:

```bash
$ wikitrivia 'art'
```

yields:

```json
[
  {
    "question": "Bennett is also an accomplished __________, having created works\u2014under the name Anthony Benedetto\u2014that are on permanent public display in several institutions.",
    "answer": "painter", "title": "Tony Bennett",
    "similar_words": ["classic", "classicist", "constructivist", "decorator", "draftsman", "etcher", "expressionist", "illustrator"]
  },
  {
    "question": "He is the __________ of the Frank Sinatra School of the Arts in ..."
  }
]
```

## Getting Started

The Wikipedia Question Generator is a Python 3 project, which uses the renowned [click](http://click.pocoo.org/3/) package to expose itself as a shell command. It can be installed and used locally through either [Docker](https://www.docker.com/) or a local installation of Python 3.4.

### Installation with Docker

To use the tool without modifying it, pull the latest image from [Docker Hub](https://registry.hub.docker.com/u/atbaker/wikipedia-question-generator/):

```bash
$ docker pull atbaker/wikipedia-question-generator
```

You can then run the image with:

```bash
$ docker run atbaker/wikipedia-question-generator --help
```

For convenience, you can alias the `docker run` command:

```bash
$ alias wikitrivia='docker run atbaker/wikipedia-question-generator'
$ wikitrivia --help
```

For those wishing to contribute to the tool, clone the repo and use [Fig](http://www.fig.sh/) for a quick setup.

### Installation with Python 3.4

To install using Python, clone the repo and create a new virtual environment with pyvenv-3.4 (or virtualenv). Then, install the requirements and the NLTK corpora:

```bash
$ pyvenv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
$ python -m textblob.download_corpora
```

Install the command line tool to use the tool easily:

```bash
$ pip install -e .
```

Once installed, you can run the tool using the `wikitrivia` command.

## Advanced Usage

By default, the tool will scrape the sample articles hard-coded into the `--help` command and return its results to stdout.

### Scraping Specific Articles

You can point the tool to a specific Wikipedia page by specifying its title. Remember

to enclose multi-word titles in quotes or the tool will treat each word as a separate title:

```bash
$ wikitrivia 'William Shatner'
```

### Scraping Multiple Articles

Scrape multiple articles simultaneously by providing multiple titles:

```bash
$ wikitrivia 'Leonard Nimoy' 'George Takei' 'Nichelle Nichols'
```

### Outputting to JSON

To use the generated data elsewhere, you can output the results to a JSON file:

```bash
$ wikitrivia --output scotty.json 'James Doohan'
```

**Note:** If you're using `docker run`, by default this will save `scotty.json` inside the container. To store the file on your local machine, mount the current directory with the `-v` option or use fig, which mounts the directory as a volume automatically.

## Methodology

The Wikipedia Question Generator uses a simple yet effective methodology to generate trivia questions. This includes:

### Identifying Suitable Sentences

1. Only consider sentences in the summary section of an article, as sentences from the body often lack context.
2. Avoid using the first sentence of the summary, as it's usually too straightforward for creating interesting trivia.
3. Disregard sentences that start with an adverb, as they often depend on the context of the previous sentence.
4. Blank out the first common noun in the sentence (e.g., 'painter', 'infantryman') to create the trivia question. Proper nouns (e.g., 'Frank Sinatra', 'The White House') usually make the answer too easy to guess.
5. If the noun is part of a noun phrase, blank out the last two words of the phrase. Blanking out just one word often makes the answer too easy if the phrase is recognizable.

### Generating Decoy Answers

For sentences where just one word was blanked out, the tool uses [WordNet](http://wordnet.princeton.edu/) to find similar words to the answer (the blanked-out word). These words serve as decoy answers during the trivia game.

For example, if the correct answer is **painter**, the hypernym of painter is **artist**. The hyponyms found for **artist** appear in the `similar_words` array in the output: "classic", "classicist", "constructivist", "decorator", "draftsman", "etcher", "expressionist", "illustrator".

While the methodology has room for improvement, it achieves impressive results using [TextBlob](http://textblob.readthedocs.org/en/dev/), [NLTK](http://www.nltk.org/), and a basic understanding of NLP.

## License

The Wikipedia Question Generator is Copyright 2019–2023 12787326 Canada Inc. and is licensed under the AGPLv3: https://opensource.org/licenses/agpl-3.0. Please ensure to comply with the license requirements when using or modifying this project.
