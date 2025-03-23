# TubeData - A YouTube Data Analysis Library

[![PyPI version](https://badge.fury.io/py/tubedata.svg)](https://badge.fury.io/py/tubedata)
[![Python Versions](https://img.shields.io/pypi/pyversions/tubedata.svg)](https://pypi.org/project/tubedata/)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

A Python package for retrieving YouTube data, including video statistics, captions, and channel information. TubeData outputs results in a user-friendly pandas DataFrame format, making it ideal for data analysis workflows — especially in Jupyter Notebooks.

## Table of Contents

- [TubeData - A YouTube Data Analysis Library](#tubedata---a-youtube-data-analysis-library)
  - [Table of Contents](#table-of-contents)
  - [Features](#features)
  - [Setup](#setup)
    - [Requirements](#requirements)
    - [API Key Setup](#api-key-setup)
      - [Setting as Environment Variable](#setting-as-environment-variable)
    - [Installation](#installation)
  - [Usage](#usage)
    - [Basic Search](#basic-search)
    - [Working with Captions](#working-with-captions)
    - [Channel Search](#channel-search)
    - [Channel Information](#channel-information)
  - [Parameter Reference](#parameter-reference)
    - [Search Class](#search-class)
    - [ChannelInfo Class](#channelinfo-class)
  - [Applications](#applications)
  - [Contributing](#contributing)
  - [License](#license)

## Features

- 🔍 **YouTube Search**: Query and retrieve results in DataFrame format
- 📊 **Video Statistics**: View counts, likes and comments count
- 📝 **Caption Extraction**: Extract video captions in multiple languages
- 📺 **Channel Information**: Data collection from specific channels

## Setup

### Requirements

- Python 3.6+
- YouTube Data API key
- Required dependencies are installed automatically

### API Key Setup

To use `tubedata`, create a YouTube Data API key following the [official Google documentation](https://developers.google.com/youtube/registering_an_application?hl=en).

#### Setting as Environment Variable

**Linux**: edit ~/.profile and add:

```bash
export YOUTUBE_DEVELOPER_KEY=<YOUR_YOUTUBE_DEVELOPER_KEY>
```

**Windows**: Set via System Properties → Environment Variables (under User variables)

### Installation

```shell
pip install tubedata
```

## Usage

### Basic Search

Create a search object to retrieve video information:

```python
import tubedata as yt
tubedata_search = yt.Search("Test", developer_key=<YOUR_YOUTUBE_DEVELOPER_KEY>)
tubedata_search.df  # DataFrame with YouTube infos (likes, views, title, etc.)
```

**Results include:**

| videoId   | publishedAt               | channelId    | title                 | … | viewCount | likeCount | favoriteCount | commentCount |
|-----------|---------------------------|--------------|-----------------------|---|-----------|-----------|---------------|--------------|
| abcde1234 | 2021-06-01 10:00:00+00:00 | abcde1234abc | Video title example 1 | … | 100000    | 6000      | 0             | 200          |
| abcde1235 | 2021-06-01 11:00:00+00:00 | abcde1234abc | Video title example 2 | … | 200000    | 5000      | 1             | 210          |
| abcde1236 | 2021-06-01 12:00:00+00:00 | abcde1234abd | Video title example 3 | … | 100000    | 4000      | 0             | 150          |

### Working with Captions

To include video captions in your results, use the argument ```captions=True```:

```python
import tubedata as yt
# YOUTUBE_DEVELOPER_KEY is not necessary if set as environment variable
tubedata_search = yt.Search("Test", caption=True)
tubedata_search.df  # A new column with captions "video_caption" will appear
```

**Results with captions:**

| videoId   | publishedAt               | channelId    | title                 | … | commentCount | video_caption                                            |
|-----------|---------------------------|--------------|-----------------------|---|--------------|----------------------------------------------------------|
| abcde1234 | 2021-06-01 10:00:00+00:00 | abcde1234abc | Video title example 1 | … | 200          | What they say; words and more words; thanks for watching |
| abcde1235 | 2021-06-01 11:00:00+00:00 | abcde1234abc | Video title example 2 | … | 210          | None                                                     |
| abcde1236 | 2021-06-01 12:00:00+00:00 | abcde1234abd | Video title example 3 | … | 150          | Words and more words and more words; thanks for watching |
| …         | …                         | …            | …                     | … | …            | …                                                        |

### Channel Search

To search for channels instead of videos:

```python
import tubedata as yt
tubedata_search = yt.Search("Test", item_type="channel")
tubedata_search.df  # DataFrame with YouTube channel information
```

**Channel search results:**

| channelId    | publishedAt               | title                  | description                      | channelTitle         | publishTime                |
|--------------|---------------------------|------------------------|----------------------------------|----------------------|----------------------------|
| abcde1234abc | 2021-06-01 10:00:00+00:00 | Example channel 1      | Description of example channel 1 | Example channel 1    | 2021-06-01 10:00:00+00:00 |
| abcde1234abd | 2021-06-01 11:00:00+00:00 | Example channel 2      | Description of example channel 2 | Example channel 2    | 2021-06-01 11:00:00+00:00 |

### Channel Information

To get information and captions from videos of specific channel(s), use the `ChannelInfo` class:

```python
import tubedata as yt
channel_info = yt.ChannelInfo(
    channel_ids=["<A CHANNEL ID>"],
    max_results=10,
    accepted_caption_lang=['pt', 'en'],
)
channel_info.df  # DataFrame with video information and captions
```

**Channel information results:**

| channelId           | videoId     | title                 | publishedAt               | caption                                                 | thumbnailUrl                    |
|---------------------|-------------|------------------------|---------------------------|--------------------------------------------------------|----------------------------------|
| EXAMPLE_CHANNEL_ID1 | EXAMPLE_VIDEO_ID1 | Example Video Title 1 | 2025-03-22 22:00:39+00:00 | Example caption text; More example text; Thanks... | https://example.com/sddefault.jpg |
| EXAMPLE_CHANNEL_ID1 | EXAMPLE_VIDEO_ID2 | Example Video Title 2 | 2025-03-22 18:00:22+00:00 | Example caption text; Follow us on social media... | https://example.com/maxresdefault.jpg |

## Parameter Reference

### Search Class

The `Search` class accepts the following arguments:

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| term | string | Yes | - | YouTube search term |
| caption | boolean | No | False | Whether to include video captions |
| maxres | integer | No | 50 | Maximum number of results to return |
| accepted_caption_lang | list | No | ['en'] | List of accepted languages for captions |
| item_type | string | No | "video" | Type of search: "video" or "channel" |
| developer_key | string | No | - | YouTube API key (optional if set as environment variable) |

Example with all parameters:

```python
import tubedata as yt

tubedata_search = yt.Search(
    term="Python Tutorial",
    caption=True,
    maxres=100,
    accepted_caption_lang=['pt', 'en'],
    item_type="video",
    developer_key="<YOUR_DEVELOPER_KEY>"
)

# Access the resulting DataFrame
df = tubedata_search.df
```

### ChannelInfo Class

The `ChannelInfo` class accepts the following arguments:

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| channel_ids | string/list | Yes | - | Channel ID or list of channel IDs |
| max_results | integer | No | 10 | Maximum number of results per channel |
| accepted_caption_lang | list | No | ['pt', 'en'] | List of accepted languages for captions |
| developer_key | string | No | - | YouTube API key (optional if set as environment variable) |

Example with all parameters:

```python
import tubedata as yt

channel_info = yt.ChannelInfo(
    channel_ids=["<CHANNEL ID 1>", "<CHANNEL ID 2>"],
    max_results=20,
    accepted_caption_lang=['pt', 'en', 'es'],
    developer_key="<YOUR_DEVELOPER_KEY>"
)

# Access the resulting DataFrame
df = channel_info.df
```

## Applications

TubeData is particularly useful for:

- **Sentiment Analysis**: Extract captions for sentiment analysis
- **Text Mining**: Identify keywords and topics from YouTube channels
- **Academic Research**: Dataset creation for video engagement studies
- **Content Marketing**: Channel performance analysis and strategy optimization
- **Competitor Research**: Tracking metrics of competitor channels

## Contributing

Contributions are welcome! Open an issue or submit a pull request on GitHub.

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
