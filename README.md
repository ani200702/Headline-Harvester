# Headline-Harvester
A lightweight Python command-line interface (CLI) that aggregates and displays real-time top stories from The Times of India. This tool allows users to quickly browse headlines and read article summaries without leaving the terminal.

FEATURES:

Real-time Aggregation: Fetches the latest news using RSS feeds.
Interactive Menu: Numbered list of top stories for easy navigation.
Article Summaries: Displays a concise summary and direct link for each selected story.
Browser Integration: Option to open the full article directly in your default web browser.

TECH STACK:

Language: Python 3.x
Libraries: feedparser (for RSS parsing), webbrowser (for opening links)

USAGE:

Run the script: python news_app_backend_TOI.py.
Browse the numbered headlines.
Enter the article number to read the summary or 'q' to quit.
[news_app_backend_TOI.py](https://github.com/user-attachments/files/25398439/news_app_backend_TOI.py)
import feedparser
import webbrowser
import os

TOI_RSS_URL = "http://timesofindia.indiatimes.com/rssfeedstopstories.cms"

def clear_screen():
    """Clears the terminal screen."""
    os.system('cls' if os.name == 'nt' else 'clear')

def get_news():
    """Fetches and parses the news from the RSS feed."""
    try:
        news_feed = feedparser.parse(TOI_RSS_URL)
        return news_feed.entries
    except Exception as e:
        print(f"An error occurred while fetching the news: {e}")
        return None

def display_headlines(articles):
    """Displays a numbered list of news headlines."""
    clear_screen()
    print("==========================================")
    print("   The Times of India - Top Stories")
    print("==========================================")
    if not articles:
        print("No news articles found.")
        return
    for i, article in enumerate(articles, 1):
        print(f"{i}. {article.title}")
    print("==========================================")

def display_article_details(article):
    """Displays the details of a selected article."""
    clear_screen()
    print("------------------------------------------")
    print(f"Title: {article.title}")
    print("------------------------------------------")
    if 'summary' in article:
        print(f"Summary: {article.summary}")
    print(f"Link: {article.link}")
    print("------------------------------------------")

def main():
    """The main function to run the news app."""
    articles = get_news()
    if not articles:
        return

    while True:
        display_headlines(articles)
        try:
            choice = input("Enter the number of the article to read (or 'q' to quit): ")

            if choice.lower() == 'q':
                break
            
            article_index = int(choice) - 1

            if 0 <= article_index < len(articles):
                selected_article = articles[article_index]
                display_article_details(selected_article)
                
                open_in_browser = input("Do you want to open the full article in your browser? (y/n): ")
                if open_in_browser.lower() == 'y':
                    webbrowser.open(str(selected_article.link))
                
                input("\nPress Enter to return to the headlines...")

            else:
                input("Invalid article number. Press Enter to try again...")

        except ValueError:
            input("Invalid input. Please enter a number. Press Enter to continue...")
        except Exception as e:
            input(f"An error occurred: {e}. Press Enter to continue...")

if __name__ == "__main__":
    main()
