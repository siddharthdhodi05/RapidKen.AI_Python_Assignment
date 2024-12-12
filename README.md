# YouTube Timestamp Extractor

This project extracts timestamps from a YouTube video's description using Selenium and saves them into a text file. The extracted timestamps are stored in `timestamps.txt` along with the video URL.

## Features
- Automatically retrieves the video description.
- Extracts all timestamps from the description using regular expressions.
- Saves the video URL and timestamps to a text file.

## Technologies Used
- **Python**: For scripting and automation.
- **Selenium**: To interact with the YouTube video page and retrieve the description.
- **Regex**: To extract timestamps from the description text.

## Prerequisites
1. Python installed on your system.
2. Selenium library installed (`pip install selenium`).
3. Chrome browser and the corresponding ChromeDriver.

## Project Code

```python
import re
import os
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Set up Chrome WebDriver options
chrome_options = Options()
chrome_options.add_argument('--headless')  # Run in headless mode
chrome_options.add_argument('--no-sandbox')
chrome_options.add_argument('--disable-dev-shm-usage')

# Initialize WebDriver
driver = webdriver.Chrome(options=chrome_options)

# YouTube video URL
url = "https://youtu.be/iTmlw3vQPSs"
driver.get(url)

try:
    # Wait for the description expander button and click it
    expand_button = WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.CSS_SELECTOR, '#description-inline-expander'))
    )
    expand_button.click()

    # Wait for the expanded description text to load
    description_element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.CSS_SELECTOR, '#description-inline-expander[is-expanded]'))
    )
    description_text = description_element.text
except Exception as e:
    print("Error extracting description:", e)
    description_text = ""

# Close the WebDriver
driver.quit()

# Extract timestamps using regex
timestamp_list = re.findall(r'\b\d{1,2}:\d{2}\b', description_text)

# Save output to a text file
output_filename = "timestamps.txt"
with open(output_filename, "w") as file:
    file.write(f"URL: {url}\n")
    file.write("Timestamps:\n")
    file.write("\n".join(timestamp_list))

print(f"Output saved to {output_filename}")
print(timestamp_list)
```

## Explanation of the Code

1. **Setting up Selenium**:
   - Chrome is run in headless mode to enable background execution.
   - `Options` are added for performance and compatibility.

2. **Navigating to the YouTube Video**:
   - The `webdriver` opens the provided video URL.

3. **Extracting the Description**:
   - Selenium waits for the "description expander" element to appear and clicks it.
   - Once expanded, the description text is fetched using CSS selectors.

4. **Extracting Timestamps**:
   - Regular expressions (`re.findall`) identify and extract all timestamps in `HH:MM` format from the description text.

5. **Saving Output**:
   - The video URL and extracted timestamps are saved into a file named `timestamps.txt`.

## Example Output
- **File Content (timestamps.txt)**:
  ```
  URL: https://youtu.be/iTmlw3vQPSs
  Timestamps:
  00:01
  00:03
  ```

- **Console Output**:
  ```
  Output saved to timestamps.txt
  ['00:01', '00:03']
  ```

## Usage
1. Replace the `url` variable with your desired YouTube video URL.
2. Run the script.
3. Check the `timestamps.txt` file for the output.

## Note
- Ensure the YouTube video has timestamps in the description for meaningful results.
- Install the correct version of ChromeDriver matching your Chrome browser version.

## Future Enhancements
- Add support for processing multiple URLs.
- Improve error handling and logging.

---
Enjoy extracting timestamps effortlessly! 🎥
