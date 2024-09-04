# research-trend-web-scrapper
Here is [link to medium article](https://medium.com/@sushant.pargaonkar97/web-scraping-machine-learning-topics-from-papers-with-code-a-practical-guide-part-1-bcecdec265fb) for detailed explanation.

Machine learning research is evolving rapidly, with new methods and papers being published every day. However, keeping track of these developments can be challenging. In this article, we'll tackle this problem by exploring how to perform web scraping on the Papers with Code website. By the end, you'll have the tools to automatically extract valuable insights about machine learning topics, methods, and associated papers.

- `requests`: This library allows us to send HTTP requests to the web server and retrieve the webpage's content.
- `BeautifulSoup`: A powerful tool for parsing HTML and XML documents, BeautifulSoup helps us navigate and search the structure of a webpage.
- `pandas`: A versatile library for data manipulation and analysis, pandas enables us to organize the scraped data into structured DataFrames for easy analysis.

While we'll use `BeautifulSoup` for this guide, other popular tools like Scrapy and Selenium offer advanced features for more complex scraping tasks.

First, let's navigate to the page that lists various machine learning methods. We'll send a GET request to retrieve the HTML content of this page.

```python
url = "https://paperswithcode.com/methods"
response = requests.get(url)  # Send an HTTP GET request to the URL
soup = BeautifulSoup(response.content, 'html.parser')  # Parse the HTML content with BeautifulSoup


Now, let's dive into the core of our scraping process—the `get_topic_method_info` function. This function is responsible for navigating to each topic's page and extracting detailed information about the methods listed.

```python
def get_topic_method_info(topic_name, card, methods_list):
    # Construct the full URL to the topic's page
    topic_link = "https://paperswithcode.com" + card.find('a')['href']

    # Send a GET request to the topic's page
    topic_response = requests.get(topic_link)
    topic_soup = BeautifulSoup(topic_response.content, 'html.parser')

    # Extract the topic's description (if available)
    description = topic_soup.find('div', class_='description-content')
    description = description.text.strip() if description else None

    # Find the table containing method details
    method_content_div = topic_soup.find('div', class_='method-content')
    table = method_content_div.find('table')

    # Extract each method's details from the table
    rows = table.find('tbody').find_all('tr')
    for row in rows:
        cols = row.find_all('td')
        method_name = cols[0].text.strip()
        num_papers = cols[1].text.strip()
        ...

    methods_list.append({...})
```end


After extracting the information, we organize it into a pandas DataFrame. Here's a quick preview of what the DataFrame looks like:

```python
df = pd.DataFrame(extracted_info)
df.head()  # Display the first few rows of the DataFrame

By leveraging BeautifulSoup for HTML parsing and pandas for data organization, we can efficiently extract and analyze the vast landscape of machine learning research. This approach opens up possibilities for automated trend analysis, research topic exploration, and even personalized research recommendations.

For those looking to expand on this work, consider integrating this data with machine learning models to predict emerging trends or using visualization tools like Matplotlib or Seaborn to create insightful graphs and charts.


