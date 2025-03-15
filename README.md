# Shazam Top 200 U.S. Weekly Songs Scraper [as of 3.10.25]

## Project Overview
This project scrapes Shazam's weekly Top 200 chart for the United States, collecting both the basic song information and the "Featured In" data for each song (albums and playlists where the song appears). My original proposal also included scraping the "Similar Songs" and "Credits" section, but it was beyond the scope of the project due to the sheer amount of data. 

## Data Collected
- This scraper collects song titles, artists, and rankings.
- For each song, it gathers all albums and playlists where the song is featured.
- The data is saved in both separate files and a combined report.

## Process

### 1. Basic Song Information Scraping
The `get_song_urls()` function collects essential song data:
- It extracts song titles from the main chart page.
- It identifies artist names for each song.
- It records song rankings from 1-200.
- It captures URLs for each song's dedicated page.

This function uses BeautifulSoup to parse the main Shazam chart page and outputs the data to `songs.txt`.

### 2. "Featured In" Data Collection
The `get_featured_in()` function:
 It takes a song URL as input for processing.
- It uses Selenium with a headless Chrome browser to load the song's page (necessary because the content loads dynamically).
- It extracts all albums and playlists where the song appears.
- It collects the type (ALBUM/PLAYLIST), title, and creator of each item.

The `scrape_shazam_top_200()` function:
- It iterates through all 200 songs in the chart.
- It calls `get_featured_in()` for each song to get detailed data.
- It writes the complete data to `songs.csv`.

### 3. Data Combination
The `create_sectioned_text_file()` function:
- It reads both the `songs.txt` and `songs.csv` files.
- It creates a combined report `shazam_data.txt` with clearly marked sections.
- The first section contains all basic song information.
- The second section contains all "Featured In" data for reference.
    - For each song, there were 11 playlists or albums the song was "Featured In" hence the repetition. 

## Challenges 

1. **Dynamic Content Loading**: The "Featured In" section on Shazam's song pages loads dynamically with JavaScript. Using regular requests wasn't sufficient because the HTML received didn't contain this data.
   - **Solution**: I used Selenium with a headless Chrome browser to load the full page, including JavaScript-rendered content.

2. **Rate Limiting**: Scraping too quickly resulted in temporary blocks from the server.
   - **Solution**: I added a delay between requests (`time.sleep(1)`) to avoid overwhelming the server.

3. **Output File**: I wanted to present the data with "tabs", but that format is not supported.
   - **Solution**: I created a sectioned text file with clear dividers and headers to organize the data.
