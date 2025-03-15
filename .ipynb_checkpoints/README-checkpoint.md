# Shazam Top 200 U.S. Weekly Songs Scraper [as of 3.10.25]

## Project Overview
This project scrapes Shazam's weekly Top 200 chart for the United States, collecting both the basic song information and the "Featured In" data for each song (albums and playlists where the song appears).

## Data Collected
- Song titles, artists, and rankings
- For each song: all albums and playlists where the song is featured
- Data saved in separate files and a combined file

## Process

### 1. Basic Song Information Scraping
The `get_song_urls()` function collects:
- Song titles
- Artist names
- Song rankings (1-200)
- URLs for each song's dedicated page

This function uses BeautifulSoup to parse the main Shazam chart page and outputs the data to `songs.txt`.

### 2. "Featured In" Data Collection
The `get_featured_in()` function:
- Takes a song URL as input
- Uses Selenium with a headless Chrome browser to load the song's page (needed because the content loads dynamically)
- Extracts all albums and playlists where the song appears
- Collects the type (ALBUM/PLAYLIST), title, and creator label for each item

The `scrape_shazam_top_200()` function:
- Iterates through all 200 songs
- Calls `get_featured_in()` for each song
- Writes the data to `songs.csv`

### 3. Data Combination
The `create_sectioned_text_file()` function:
- Reads both `songs.txt` and `songs.csv`
- Creates a combined file `shazam_data.txt` with clearly marked sections
- First section contains basic song information
- Second section contains all "Featured In" data

## Challenges Encountered

1. **Dynamic Content Loading**: The "Featured In" section on Shazam's song pages loads dynamically with JavaScript. Using regular requests wasn't enough because the HTML received didn't contain this data.
   - **Solution**: Used Selenium with a headless Chrome browser to load the full page, including JavaScript-rendered content.

2. **Rate Limiting**: Scraping too quickly resulted in temporary blocks.
   - **Solution**: Added a delay between requests (`time.sleep(1)`) to avoid overwhelming the server.

3. **Data Storage**: Wanted to present the data with "tabs" in a text file format.
   - **Solution**: Created a sectioned text file with clear dividers and headers to organize information.

## Usage
Run the script to collect the current Shazam Top 200 chart data and generate the combined report:

```python
python shazam_scraper.py
```

## Output Files
- `songs.txt`: Basic information about the Top 200 songs
- `songs.csv`: Detailed "Featured In" data for each song
- `shazam_data.txt`: Combined report with both datasets